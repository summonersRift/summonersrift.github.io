---
layout: post
title:  "LLVM insert instruction to each basic block"
date:   2018-07-11
---

<p class="intro"><span class="dropcap">L</span>LVM for advanced grad students..</p>
LLVM is a machine independent intermediate representation of an application source. Program compilers works in passes. We can design very advanced passes using LLVM. LLVM is very popular for performing optimizations. 
There arent enough examples in the open web if you want to do some advanced stuf. 

A good place to start-- 
1. https://github.com/abenkhadra/llvm-pass-tutorial
2. https://www.cs.cornell.edu/~asampson/blog/llvm.html


<b>REMEMBER—</b> use the LLVM <i>doxygen</i> refernces if thigs get compilcated.
There are issues with LLVM versions as well. Make sure you ahve same version of LLVM, Clang and llc and other tools. If you are on ubuntu search with <i>apt-cache search llvm</i>, then install llvm-6.0 and such.

<b>Commands—</b>
{% highlight bash %}
make
clang-6.0 -S -emit-llvm -Xclang -load -Xclang skeleton/libSkeletonPass.so test.c 
llc-6.0 test.ll
rm test a.out  test.s
llc-6.0 test.ll
clang-6.0 test.s
./a.out 
{% endhighlight %}


<b>Source Code of the Pass(es)— </b>
{% highlight cpp %}

#include "llvm/Pass.h"
#include "llvm/IR/Function.h"
#include "llvm/IR/BasicBlock.h"
#include "llvm/IR/IRBuilder.h"
#include "llvm/Support/raw_ostream.h"
#include "llvm/IR/LegacyPassManager.h"
#include "llvm/Transforms/IPO/PassManagerBuilder.h"

using namespace llvm;

uint32_t block_uid=1;

Function *AedemFunc;
namespace {
  struct MyFunctionPass : public FunctionPass {
    static char ID;
    MyFunctionPass() : FunctionPass(ID) {}

    virtual bool runOnFunction(Function &F) {
	errs() << " * ["<<ID<<"] runOnFunction() encountered=" << F.getName() << "!\n";
	if (F.getName() == "aedem_block"){
        errs()<< "     AedemFunc="<<F.getName() <<"\n";
	AedemFunc = &F;
    	//body"<<F<<"\n";
      }
      return false;
    }
  };

  struct MyBBPass01 : public BasicBlockPass {
    static char ID;
    MyBBPass01() : BasicBlockPass(ID) {}


    virtual bool runOnBasicBlock(BasicBlock &BB) {
      Function *myFunc = BB.getParent();
      if (myFunc->getName() == "aedem_block"){
         errs() <<"-->function aedem_block(), no marker insertion\n";
         return true;
      }

      //else

      errs() << " * Encountered a basic block \'" << BB.getName() 
	      << "\', total instructions = " << BB.size() << "\n";
      //get first instruction
      
      //BasicBlock::iterator pi;
      Instruction* fi = BB.getFirstNonPHI();
      //const char *getOpcodeName()      //fi = BB.getInstList().first();
      errs() << "   BB first NonPHI instruction: \'"<<fi->getOpcodeName()
	      <<"\', block_uid="<<block_uid++<<"\n";

      IRBuilder<> builder(&BB);
      Module *mymod = BB.getModule();
      Function *myHookFunc = mymod->getFunction("aedem_block");
      //errs()<< "     myfunc="<<myHookFunc->getName()
      //      <<"\n    body"<<*myHookFunc<<"\n";
      Value *bid = llvm::ConstantInt::get(Type::getInt32Ty(BB.getContext()),block_uid++);
      //Value *ret = builder.CreateCall(myHookFunc, bid, "myHookFunc");
      Value *ret = builder.CreateCall(myHookFunc, bid, "myHookFunc");
      //CallInst* callOne;
      //Instruction* newInst = CallInst::Create(myHookFunc, bid, "");
      //fi->getParent()->getInstList().insert(fi->getIterator(), newInst);

      //Function *myDummyFunc = mymod->getFunction("dummyfunc");
      //errs()<< "     myDummyFunc="<<myDummyFunc->getName()
      //      <<"\n    body="<<*myDummyFunc<<"\n";
      //Instruction *newInst = CallInst::Create(myDummyFunc, "");
      //
      
     //segmentation fault caused by recursive calls

      BasicBlock *b = &BB;
      for(BasicBlock::iterator BI = b->begin(), BE = b->end(); BI != BE; ++BI)
      {
         if(isa<CallInst>(&(*BI)) )
         {  
            //check inst -- print
            //errs() << "    CallInst detected-"<<BI->getOpcodeName()
            //       << ", num-operand="<<BI->getNumOperands()<<"\n";
	    
	    if (BI->getOperand(1)->getName() == "aedem_block"){
	        errs() << "        func="<<BI->getOperand(1)->getName()<<"\n";
	        errs() << "          moving this call up..\n";
               
                Instruction *newInst = (Instruction *)&(*BI);
	        errs() << "           *"<<*newInst<<"\n";
                newInst->moveBefore(fi);
	    }
         }
      }
      return true;
    }
  };



}

char MyFunctionPass::ID = 1;
char MyBBPass01::ID = 2;

//static RegisterPass<MyBBPass> X ("MyBBPass", "test fuction exist", false, false);

//static RegisterSPass  RegisterMyPass(PassManagerBuilder::EP_EarlyAsPossible,
//               registerSkeletonPass);


// Automatically enable the pass.
// http://adriansampson.net/blog/clangpass.html
static void registerMyBBPass01(const PassManagerBuilder &,
                         legacy::PassManagerBase &PM) {
  //PM.add(new MyFunctionPass());
  PM.add(new MyBBPass01());
}
static RegisterStandardPasses
  RegisterMyPass(PassManagerBuilder::EP_EarlyAsPossible,
                 registerMyBBPass01);

/*
static void registerSkeletonPass(const PassManagerBuilder &,
                         legacy::PassManagerBase &PM) {
  PM.add(new SkeletonPass());
}
static RegisterStandardPasses
  RegisterMyPass(PassManagerBuilder::EP_EarlyAsPossible,
                 registerSkeletonPass);

*/
{% endhighlight %}

<b>Sample output—</b>
<code>
user@callisto:~/llvm-example/llvm-pass-basicblock/build$ clang-6.0 -S -emit-llvm -Xclang -load -Xclang skeleton/libSkeletonPass.so test.c 
-->function aedem_block(), no marker insertion
 * Encountered a basic block '', total instructions = 15
   BB first NonPHI instruction: 'alloca', block_uid=1
        func=aedem_block
          moving this call up..
           *  %11 = call i32 @aedem_block(i32 2)
 * Encountered a basic block '', total instructions = 5
   BB first NonPHI instruction: 'load', block_uid=3
        func=aedem_block
          moving this call up..
           *  %16 = call i32 @aedem_block(i32 4)
 * Encountered a basic block '', total instructions = 5
   BB first NonPHI instruction: 'load', block_uid=5
        func=aedem_block
          moving this call up..
           *  %21 = call i32 @aedem_block(i32 6)
 * Encountered a basic block '', total instructions = 3
   BB first NonPHI instruction: 'load', block_uid=7
        func=aedem_block
          moving this call up..
           *  %25 = call i32 @aedem_block(i32 8)
(failed reverse-i-search)`llm': clang-6.0 -S -emit-^Cvm -Xclang -load -Xclang skeleton/libSkeletonPass.so test.c 
</code>
