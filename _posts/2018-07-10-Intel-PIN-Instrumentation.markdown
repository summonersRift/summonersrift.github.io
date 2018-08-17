---
layout: post
title: Intel PIN Instrumentation
published: true
---

<p class="intro">PIN is very useful for instrumenting a program dynamically. PIN can instrument the program without modifying its behavior. here aren't enough examples in the open web if you want to do advanced optimization.
</p>


Install the following libraries to run PIN examples or an error might occur.
{% highlight cpp %}
sudo apt-get install gcc-multilib g++-multilib
{% endhighlight %}


### Execution
{% highlight cpp %}
cd ~/pin-3.7/source/tools/ManualExamples
make all TARGET=ia32
make proccount.test TARGET=intel64

../../../pin -t obj-intel64/proccount.so -- ~/thesis/aedem/laplace2d64int proccount.cpp Makefile
{% endhighlight %}

### Code
{% highlight cpp %}
/* compile----
make proccount.test TARGET=intel64

run:
../../../pin -t obj-intel64/proccount.so -- ~/thesis/aedem/laplace2d64int proccount.cpp Makefile
*/

// This tool counts the number of times a routine is executed and 
// the number of instructions executed in a routine

#include <fstream>
#include <iomanip>
#include <iostream>
#include <string.h>
#include "pin.H"

ofstream outFile;
static UINT64 icount = 0; //static for compiler performance optimization
static UINT64 icount_dup = 0; //duplicate to verify
static UINT64 rcount = 0; //load/read count
static UINT64 wcount = 0; //store/write count
static int block_id;

struct node {
   //char *block_uid;
   UINT64 uid;         //block uid
   UINT64 ins;  //num instuctions in a block
   UINT64 total;     //total nstructions so far
   struct node *next;
};
struct node *head = NULL;

void insert_node(int call_num){//, char *block_uid) {   //insert link at the first location
   //create a link
   //char *copy_block_uid;
   //copy_block_uid = (char*) malloc(sizeof(char) * (strlen(block_uid) + 1));
   //strcpy(copy_block_uid, block_uid);

   struct node *link = (struct node*) malloc(sizeof(struct node));
   link->uid = call_num;   904  vim pinatrace.cpp
  905  make pinatrace.test TARGET=intel64
  906  ../../../pin -t obj-intel64/pinatrace.so -- /home/mobai001/llvm/llvm-pass-tutorial/build/jacobi.out pinatrace.cpp Makefile
  907  ls
  908  vim inscount.out 

   link->total = icount; //global variable instuction count
   //point it to old first node
   if (head){
       head->ins =  icount - head->total;}
   link->next = head;
   //point first to new first node
   head = link;
}
struct node* reverse_list(struct node* ptr){//, char *block_uid) {   //insert link at the first location
   struct node* ptr_prev = NULL;
   while (ptr != NULL){
      struct node* temp = ptr->next;
      ptr->next = ptr_prev;
      ptr_prev = ptr;
      ptr = temp;
   }
   return ptr_prev;
}

// Holds instruction count for a single procedure
typedef struct RtnCount
{
    string _name;
    string _image;
    ADDRINT _address;
    RTN _rtn;
    UINT64 _rtnCount;
    UINT64 _icount;
    struct RtnCount * _next;
} RTN_COUNT;

// Linked list of instruction counts for each routine
RTN_COUNT * RtnList = 0;

// This function is called before every instruction is executed
VOID docount(UINT64 * counter) {
    (*counter)++;
}

VOID DoLoad(REG reg, ADDRINT * addr) {
   rcount += 1;
}
VOID DoStore(REG reg, ADDRINT * addr) {
   wcount += 1;
}


//--------------------------------------------------------------
VOID inscounter() { icount++;}
VOID Instruction(INS ins, VOID *v) {
   icount_dup += 1;
   // Insert a call to docount before every instruction, no arguments are passed
   INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)inscounter, IARG_END);

   //if (INS_IsMemoryRead(ins)){
   //   rcount += 1;
   //}

    //insert a call before evert memory read
    if (INS_Opcode(ins) == XED_ICLASS_MOV &&
        INS_IsMemoryRead(ins) &&
        INS_OperandIsReg(ins, 0) &&
        INS_OperandIsMemory(ins, 1))
    {
        // op0 <- *op1
        INS_InsertCall(ins, IPOINT_BEFORE, AFUNPTR(DoLoad),
                       IARG_UINT32,
                       REG(INS_OperandReg(ins, 0)),
                       IARG_MEMORYREAD_EA,
                       IARG_END);
    }
    //insert a call before evert memory write
    else if (INS_Opcode(ins) == XED_ICLASS_MOV &&
        INS_IsMemoryWrite(ins) &&
        INS_OperandIsReg(ins, 1) &&
        INS_OperandIsMemory(ins, 0))
    {
        // op0 <- *op1
        INS_InsertCall(ins, IPOINT_BEFORE, AFUNPTR(DoStore),
                       IARG_UINT32,
                       REG(INS_OperandReg(ins, 0)),
                       IARG_MEMORYWRITE_EA,
                       IARG_END);
    }


}
//--------------------------------------------------------------

UINT64 aedem_counter=0;

//VOID aedem_block_detected(UINT64 *calls)
VOID aedemBlockHandler(char *name, int num)
{
   aedem_counter += 1;
   //do a linked list addition here
   insert_node(num);
   //save the block number  to a global variable
   block_id = num;
    
}


const char * StripPath(const char * path)
{
    const char * file = strrchr(path,'/');
    if (file)
        return file+1;
    else
        return path;
}



// Pin calls this function every time a new rtn is executed, once per routine
VOID Routine(RTN rtn, VOID *v)
{
    
    // Allocate a counter for this routine
    RTN_COUNT * rc = new RTN_COUNT;

    // The RTN goes away when the image is unloaded, so save it now
    // because we need it in the fini
    rc->_name = RTN_Name(rtn);
    rc->_image = StripPath(IMG_Name(SEC_Img(RTN_Sec(rtn))).c_str());
    rc->_address = RTN_Address(rtn);
    rc->_icount = 0;
    rc->_rtnCount = 0;

    // Add to list of routines
    rc->_next = RtnList;
    RtnList = rc;
            
    RTN_Open(rtn);
            
    // Insert a call at the entry point of a routine to increment the call count
    RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR)docount, IARG_PTR, &(rc->_rtnCount), IARG_END);
 
    

      //RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR)aedem_block_detected, IARG_PTR, &(rc->_rtnCount), IARG_END);
      /*https://stackoverflow.com/questions/7896581/intel-pin-rtn-insertcall-multiple-function-arguments*/
      RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR)aedemBlockHandler, 
                IARG_ADDRINT, "aedem_block", IARG_FUNCARG_ENTRYPOINT_VALUE, 
                    0, IARG_END);
      /*
      RTN_InsertCall(funcRtn, IPOINT_BEFORE, (AFUNPTR)funcHandler, 
                IARG_ADDRINT, "funcA", IARG_FUNCARG_ENTRYPOINT_VALUE, 
                    0, IARG_FUNCARG_ENTRYPOINT_VALUE, 1,
                        IARG_FUNCARG_ENTRYPOINT_VALUE, 2, IARG_END);
      */
      //RTN_InsertCall(rtn, IPOINT_AFTER, (AFUNPTR)aedem_block_detected, IARG_UINT64,
      //      IARG_FUNCRET_EXITPOINT_VALUE, IARG_END);
    }

    // For each instruction of the routine
    for (INS ins = RTN_InsHead(rtn); INS_Valid(ins); ins = INS_Next(ins))
    {
        // Insert a call to docount to increment the instruction counter for this rtn
        INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)docount, IARG_PTR, &(rc->_icount), IARG_END);
    }
    RTN_Close(rtn);
}

// This function is called when the application exits
// It prints the name and count for each procedure
VOID Fini(INT32 code, VOID *v)
{
    outFile << setw(23) << "Procedure" << " "
          << setw(15) << "Image" << " "
          << setw(18) << "Address" << " "
          << setw(12) << "Calls" << " "
          << setw(12) << "Instructions" << endl;

    for (RTN_COUNT * rc = RtnList; rc; rc = rc->_next)
    {
        if (rc->_icount > 0)
            outFile << setw(23) << rc->_name << " "
                  << setw(15) << rc->_image << " "
                  << setw(18) << hex << rc->_address << dec <<" "
                  << setw(12) << rc->_rtnCount << " "
                  << setw(12) << rc->_icount << endl;
    }

    outFile <<"###1----------------------------------------------------"<<endl;
    outFile <<"# Total occurance of aedem_block:" << aedem_counter<<endl;
    outFile <<"# Instructions icount="<<icount<<" ,icount_dup="<<icount_dup<<endl;
    outFile <<"# Load  rcount="<<rcount<<endl;
    outFile <<"# Store wcount="<<wcount<<endl;
    outFile <<"###2----------------------------------------------------"<<endl;
    
    outFile <<"block-uid,instuctions,total-instructions"<<endl;
    head = reverse_list(head);
    for (node *block = head; block; block = block->next)
    {
       outFile <<block->uid<<","<< block->ins<<","<< block->total<<endl;
      //calls += 1;
    }
    outFile <<"###f---------------------------------------------------"<<endl;
    //outFile <<endl<<"Total entry in linked list(node):" << calls<<endl;
}

/* ===================================================================== */
/* Print Help Message                                                    */
/* ===================================================================== */

INT32 Usage()
{
    cerr << "This Pintool counts the number of times a routine is executed" << endl;
    cerr << "and the number of instructions executed in a routine" << endl;
    cerr << endl << KNOB_BASE::StringKnobSummary() << endl;
    return -1;
}

/* ===================================================================== */
/* Main                                                                  */
/* ===================================================================== */

int main(int argc, char * argv[])
{
    // Initialize symbol table code, needed for rtn instrumentation
    PIN_InitSymbols();

    outFile.open("proccount.out");

    // Initialize pin
    if (PIN_Init(argc, argv)) return Usage();

    // Register Instruction to be called to instrument instructions
    INS_AddInstrumentFunction(Instruction, 0);

    // Register Routine to be called to instrument rtn
    RTN_AddInstrumentFunction(Routine, 0);

    // Register Fini to be called when the application exits
    PIN_AddFiniFunction(Fini, 0);
    
    // Start the program, never returns
    PIN_StartProgram();
    
    return 0;
}

{% endhighlight %}


## Example 2
Trace a programs memory accesses and specific function calls with parameters. 
{% highlight bash %}
vim pinatrace.cpp
make pinatrace.test TARGET=intel64
../../../pin -t obj-intel64/pinatrace.so -- /home/mobai001/llvm/llvm-pass-tutorial/build/jacobi.out pinatrace.cpp Makefile
vim inscount.out 
{% endhighlight %}


### Code
{% highlight cpp %}
/*This file contains an ISA-portable PIN tool for tracing memory accesses. */

#include <stdio.h>
#include "pin.H"

FILE * trace;

// Print a memory read record
VOID RecordMemRead(VOID * ip, VOID * addr){
    fprintf(trace,"%p: R %p\n", ip, addr);
}

// Print a memory write record
VOID RecordMemWrite(VOID * ip, VOID * addr){
    fprintf(trace,"%p: W %p\n", ip, addr);
}

VOID RecordBlockStart(char *name, int num){
    fprintf(trace,"bid=%d\n", num);
}

VOID RecordKernelStart(char *name, int num){
    fprintf(trace,"kid=%d\n", num);
}

// Is called for every instruction and instruments reads and writes
VOID Instruction(INS ins, VOID *v)
{
    // Instruments memory accesses using a predicated call, i.e.
    // the instrumentation is called iff the instruction will actually be executed.
    // On the IA-32 and Intel(R) 64 architectures conditional moves and REP 
    // prefixed instructions appear as predicated instructions in Pin.
    UINT32 memOperands = INS_MemoryOperandCount(ins);

    // Iterate over each memory operand of the instruction.
    for (UINT32 memOp = 0; memOp < memOperands; memOp++) {
        if (INS_MemoryOperandIsRead(ins, memOp)) {
            INS_InsertPredicatedCall(
                ins, IPOINT_BEFORE, (AFUNPTR)RecordMemRead,
                IARG_INST_PTR,
                IARG_MEMORYOP_EA, memOp,
                IARG_END);
        }
        // Note that in some architectures a single memory operand can be 
        // both read and written (for instance incl (%eax) on IA-32)
        // In that case we instrument it once for read and once for write.
        if (INS_MemoryOperandIsWritten(ins, memOp)) {
            INS_InsertPredicatedCall(
                ins, IPOINT_BEFORE, (AFUNPTR)RecordMemWrite,
                IARG_INST_PTR,
                IARG_MEMORYOP_EA, memOp,
                IARG_END);
        }
    }
}

VOID Fini(INT32 code, VOID *v){
    fprintf(trace, "#eof\n");
    fclose(trace);
}

// Pin calls this function every time a new rtn is executed
VOID Routine(RTN rtn, VOID *v){
    RTN_Open(rtn);
    if (RTN_Name(rtn).compare("aedem_block") == 0) { //check routine name is aedem_block
      //RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR)aedem_block_detected, IARG_PTR, &(rc->_rtnCount), IARG_END);
      /*https://stackoverflow.com/questions/7896581/intel-pin-rtn-insertcall-multiple-function-arguments*/
      RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR) RecordBlockStart, 
                IARG_ADDRINT, "aedem_block", IARG_FUNCARG_ENTRYPOINT_VALUE, 
                    0, IARG_END);
    }
    else if (RTN_Name(rtn).compare("aedem_kernel") == 0) { //check routine name is aedem_block
      //RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR)aedem_block_detected, IARG_PTR, &(rc->_rtnCount), IARG_END);
      /*https://stackoverflow.com/questions/7896581/intel-pin-rtn-insertcall-multiple-function-arguments*/
      RTN_InsertCall(rtn, IPOINT_BEFORE, (AFUNPTR) RecordKernelStart, 
                IARG_ADDRINT, "aedem_kernel", IARG_FUNCARG_ENTRYPOINT_VALUE, 
                    0, IARG_END);
    }
    RTN_Close(rtn);
}


/* Print Help Message */
INT32 Usage(){
    PIN_ERROR( "This Pintool prints a trace of memory addresses with a basic block uid\n" 
              + KNOB_BASE::StringKnobSummary() + "\n");
    return -1;
}


int main(int argc, char *argv[])
{
    // Initialize symbol table code, needed for rtn instrumentation
    PIN_InitSymbols();

    //Initialize PIN
    if (PIN_Init(argc, argv)) return Usage();

    //setup trace file
    trace = fopen("pinatrace.out", "w");

    // Register Instruction to be called to instrument instructions
    INS_AddInstrumentFunction(Instruction, 0);

    // Register Routine to be called to instrument rtn
    RTN_AddInstrumentFunction(Routine, 0);

    // Register Fini to be called when the application exits
    PIN_AddFiniFunction(Fini, 0);

    // Never returns
    PIN_StartProgram();
    
    return 0;
}
{% endhighlight %}


