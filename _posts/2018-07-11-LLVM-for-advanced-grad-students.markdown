---
layout: post
title: LLVM insert instruction to each basic block
published: true
---

<figure>
	<img src="https://llvm.org/img/DragonMedium.png" alt="LLVM LOGO"> 
	<figcaption>LLVM Dragon</figcaption>
</figure>


This is an advanced LLVM pass for grad students. LLVM is a machine independent intermediate representation of an application source. Program compilers works in passes. We can design very advanced passes using LLVM. LLVM is very popular for performing optimizations.
There aren't enough examples in the open web if you want to do advanced optimization. <br>

<b>REMEMBER—</b> use the LLVM <i>doxygen</i> references if things get compilcated.
There are issues with LLVM versions. Make sure you have same version of LLVM, Clang and llc and other tools.<br>
  


**Useful links**—
1. <a href="https://github.com/abenkhadra/llvm-pass-tutorial">Github Skeleton LLVM pass source code </a>
1. <a href="https://www.cs.cornell.edu/~asampson/blog/llvm.html">Adrian Sampson's LLVM resource</a>
1. <a href="https://stackoverflow.com/questions/9148890/how-to-make-clang-compile-to-llvm-ir">StackOverflow Post to on multi-file compilation</a>


{% highlight bash %}
#ubuntu: install LLVM, Clang, linker and libraries
sudo apt-cache search llvm | grep 6
sudo apt-get install clang-6.0 libclang-6.0-dev libclang-common-6.0-dev libclang1-6.0 clang-tools-6.0 clang-6.0-examples
sudo apt-get install llvm-6.0 llvm-6.0-dev lld-6.0 llvm-6.0-tools lldb-6.0 liblldb-6.0 libfuzzer-6.0-dev
sudo apt-get install cmake

#getting a skeleton pass builder source codeto start with
cd ~
mkdir llvm
cd llvm
git clone https://github.com/summonersrift/llvm-pass-tutorial
#summonersrift is my username
#git clone https://github.com/abenkhadra/llvm-pass-tutorial
cd llvm-pass-tutorial
mkdir build
cd build
cmake ..
make

#run your LLVM pass, generate binaries and execute a.out
clang-6.0 -S -emit-llvm -Xclang -load -Xclang skeleton/libSkeletonPass.so test.c 
llc-6.0 test.ll
rm test a.out  test.s
llc-6.0 test.ll
clang-6.0 test.s
./a.out 

###LLVM multi-file compilation
#a) generate all .ll files
clang-6.0 -S -emit-llvm *.c
#b) link them into a single one
llvm-link-6 -S -v -o single.ll *.ll
# (Optional) Optimise your code
opt-6.0 -S -O3 -aa -basicaaa -tbaa -licm single.ll -o optimised.ll
#c) Generate assembly (generates a optimised.s file)
llc-6.0 optimised.ll
#d) Create executable (named a.out)
clang-6.0 optimised.s
{% endhighlight %}


#### LLVM Pass Source Code
Modify the ~/llvm/llvm-pass-tutorial/skeleton/Skeleton.cpp 

{% highlight cpp %}
vim ~/llvm/llvm-pass-tutorial/skeleton/Skeleton.cpp
{% endhighlight %}

Following is the source code for the pass—
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
/* static void registerSkeletonPass(const PassManagerBuilder &,
                         legacy::PassManagerBase &PM) {
  PM.add(new SkeletonPass());
}
static RegisterStandardPasses
  RegisterMyPass(PassManagerBuilder::EP_EarlyAsPossible,
                 registerSkeletonPass);
*/
{% endhighlight %}
<b>Terminal output—</b>
<pre>
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
</pre>
  

<figure>
	<img src="https://llvm.org/img/LLVM-Logo-Derivative-1.png" alt="LLVM LOGO"> 
	<figcaption></figcaption>
</figure>
