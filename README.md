# CBT643
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 643 is from Paul A. Scott and contains some programs      *   FILE 643
//*           and macros he has written.  Documentation for these   *   FILE 643
//*           is included below.  Additionally, see members $$DOC,  *   FILE 643
//*           $$MACDOC, and $ALTINST in this pds.                   *   FILE 643
//*                                                                 *   FILE 643
//*           email:      "Paul A. Scott" <pscott@skycoast.us>      *   FILE 643
//*           web site:   http://skycoast.us/pscott/software/mvs/   *   FILE 643
//*                                                                 *   FILE 643
//*                         MVS Software                            *   FILE 643
//*                                                                 *   FILE 643
//*     Source Code                                                 *   FILE 643
//*                                                                 *   FILE 643
//*     Here you'll find a collection of useful programs I've       *   FILE 643
//*     written on IBM's MVS operating system.                      *   FILE 643
//*                                                                 *   FILE 643
//*     Macro Library  -  members are part of SRCLIB                *   FILE 643
//*                                                                 *   FILE 643
//*     My experience with the stack architecture of 8-bit          *   FILE 643
//*     microprocessors---as well as the Concept 14                 *   FILE 643
//*     macros---inspired me to write a macro library that          *   FILE 643
//*     simulates a stack architecture for S/390. The macro         *   FILE 643
//*     library is assigned the acronym PSM, which stands for       *   FILE 643
//*     Pseudo Stack Machine or Paul Scott's Macros depending       *   FILE 643
//*     on my mood. The PSM macros used in the code exhibits        *   FILE 643
//*     are:                                                        *   FILE 643
//*                                                                 *   FILE 643
//*     PENTER    --- Module entry linkage with optional stack      *   FILE 643
//*                   construction                                  *   FILE 643
//*     PEXIT     --- Module exit linkage with auto stack           *   FILE 643
//*                   destruction                                   *   FILE 643
//*     PCALL     --- Call external entry point reentrantly         *   FILE 643
//*     PUSHREG   --- Push one or more registers onto stack         *   FILE 643
//*     POPREG    --- Pop one or more registers from stack          *   FILE 643
//*                                                                 *   FILE 643
//*     JCL String Compare                                          *   FILE 643
//*                                                                 *   FILE 643
//*     PSU001 --- Sets the step condition code to the boolean      *   FILE 643
//*     result of the string comparison specified in the PARM       *   FILE 643
//*     field. This allows conditional execution of subsequent      *   FILE 643
//*     steps based on the comparison                               *   FILE 643
//*                                                                 *   FILE 643
//*     Example:                                                    *   FILE 643
//*     //STEP0001 EXEC PGM=PSU001,PARM=('&DAY,EQ,FRI')             *   FILE 643
//*     //*                                                         *   FILE 643
//*                                                                 *   FILE 643
//*                                                                 *   FILE 643
//*     JCL Instream Data                                           *   FILE 643
//*                                                                 *   FILE 643
//*     PSU002 --- Writes one or more lines of text in the PARM     *   FILE 643
//*     field to SYSOUT. The first character of the PARM field      *   FILE 643
//*     is the line separator and is not included in the            *   FILE 643
//*     output.                                                     *   FILE 643
//*                                                                 *   FILE 643
//*     Example:                                                    *   FILE 643
//*     //STEP0001 EXEC PGM=PSU002,                                 *   FILE 643
//*     //            PARM=('\',                       SEPARATOR    *   FILE 643
//*     //            ' C INDD=SYSUT1,OUTDD=SYSUT2\',  CARD #1      *   FILE 643
//*     //            ' S M=(MEMBERA,MEMBERB) ')       CARD #2      *   FILE 643
//*     //SYSOUT  DD  DSN=&&IEBCOPY,UNIT=VIO, ...                   *   FILE 643
//*     //*                                                         *   FILE 643
//*                                                                 *   FILE 643
//*     JCL Dataset Editor                                          *   FILE 643
//*                                                                 *   FILE 643
//*     PSU003 --- Copy SYSUT1 to SYSUT2 while replacing one        *   FILE 643
//*     string with another. Specify one or more "old=new"          *   FILE 643
//*     strings in the PARM field, delimited by a comma.  All       *   FILE 643
//*     occurences of old in SYSUT1 will be replaced with new       *   FILE 643
//*     in SYSUT2 during the copy operation.                        *   FILE 643
//*                                                                 *   FILE 643
//*     Example:                                                    *   FILE 643
//*     //STEP0001 EXEC PGM=PSU003,                                 *   FILE 643
//*     //            PARM=('&&PREFIX=PROD.LOAN.SL',                *   FILE 643
//*     //            '&&UNIT=SYSDA')                               *   FILE 643
//*     //SYSIN    DD  DUMMY,DCB=(RECFM=FB,LRECL=80,BLKSIZE=80)     *   FILE 643
//*     //SYSUT1   DD  DSN=PROD.CONTROL.JCL(LOAN001),DISP=SHR       *   FILE 643
//*     //SYSUT2   DD  DSN=PROD.CONTROL.JCL(LOAN001),DISP=SHR       *   FILE 643
//*     //*                                                         *   FILE 643
//*                                                                 *   FILE 643
//*     Calendar Generator                                          *   FILE 643
//*                                                                 *   FILE 643
//*     PSU004 --- Generate a calendar for the current (or          *   FILE 643
//*                specified) year.                                 *   FILE 643
//*                                                                 *   FILE 643
//*     Example:                                                    *   FILE 643
//*     //STEP0001 EXEC PGM=PSU004,PARM=('1989')                    *   FILE 643
//*     //SYSOUT    DD  SYSOUT=*                                    *   FILE 643
//*     //*                                                         *   FILE 643
//*                                                                 *   FILE 643
//*     Alternately, use a CLIST to generate a calendar in ISPF     *   FILE 643
//*     edit.  CLIST called CALENDAR is supplied here.              *   FILE 643
//*                                                                 *   FILE 643
//*                                                                 *   FILE 643
//*     Once you've had a chance to review the code above, you      *   FILE 643
//*     may want to grab the Installation Job Stream that will      *   FILE 643
//*     install all of the programs, plus an IVP and                *   FILE 643
//*     documentation. Change only the JOB statement and the        *   FILE 643
//*     parameters on the first PROC statement. The Job Stream      *   FILE 643
//*     is set up to take care of the rest.                         *   FILE 643
//*                                                                 *   FILE 643
//*     Important: The installation requires MVS/ESA or above       *   FILE 643
//*     and Assembler H.                                            *   FILE 643
//*                                                                 *   FILE 643
```
