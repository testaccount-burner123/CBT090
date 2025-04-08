# CBT090
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 090 is from David Noon and is a DELINKER program, which   *   FILE 090
//*           converts load modules into 80-byte object decks.      *   FILE 090
//*           This file was prepared for the CBT Tape by Greg       *   FILE 090
//*           Price.                                                *   FILE 090
//*                                                                 *   FILE 090
//*     This file (CBT File 90) contains the "Delinker" package     *   FILE 090
//*     written by David W. Noon which consists of source code      *   FILE 090
//*     (PL/I and Assembler) and documentation.                     *   FILE 090
//*                                                                 *   FILE 090
//*                                                                 *   FILE 090
//*     David Noon's email:  dwnoon@ibm.net      -or-               *   FILE 090
//*                       dwnoon@compuserve.com                     *   FILE 090
//*                                                                 *   FILE 090
//*     In March 1999, someone asked how to read and write          *   FILE 090
//*     RECFM=U data from PL/I in the comp.lang.pl1 USENET          *   FILE 090
//*     newsgroup.  David Noon posted that if a delinker written    *   FILE 090
//*     in PL/I was wanted then just ask.  That's not what the      *   FILE 090
//*     original poster wanted, but I asked instead, and Dave       *   FILE 090
//*     duly emailed it to me.                                      *   FILE 090
//*                                                                 *   FILE 090
//*     The package turned out to be a powerful batch utility       *   FILE 090
//*     to delink, resize and even package for later processing     *   FILE 090
//*     (such as distribution and reinstallation) some or all       *   FILE 090
//*     CSECTs of nominated load modules.  It could, for            *   FILE 090
//*     example, be used in a job stream to replace certain         *   FILE 090
//*     CSECTs with newer versions.                                 *   FILE 090
//*                                                                 *   FILE 090
//*     I ended up plugging it into REVIEW R31.0 (CBT File 134),    *   FILE 090
//*     so that members tagged in the member list (or all           *   FILE 090
//*     members if none are tagged) can be dynamically delinked.    *   FILE 090
//*                                                                 *   FILE 090
//*     Please note that this Delinker will not process             *   FILE 090
//*     scatter-load or segment-overlay programs correctly.  It     *   FILE 090
//*     will only process load modules, and not program objects.    *   FILE 090
//*                                                                 *   FILE 090
//*     In case you do not have a suitable PL/I compiler handy,     *   FILE 090
//*     I have supplied DELINKI and DWNSPDSR load modules in CBT    *   FILE 090
//*     File 135.  I proposed calling the program DELINK1 to        *   FILE 090
//*     distinguish it from the DELINK/DELINK0 OS/360 FE Tool       *   FILE 090
//*     (and its derivatives), but Sam Golob preferred DELINKI,     *   FILE 090
//*     so DELINKI it is.  (The 1 or I denotes that it is written   *   FILE 090
//*     in PL/I.)                        Greg Price, July 1999.     *   FILE 090
//*                                                                 *   FILE 090
//*                                                                 *   FILE 090
//*     Minor changes in 2006 include AMODE(64) support.            *   FILE 090
//*                                                                 *   FILE 090
//*     A member contents list follows.                             *   FILE 090
//*                                                                 *   FILE 090
//*     -MEMBER-   -CONTENTS------------------------------------    *   FILE 090
//*                                                                 *   FILE 090
//*     $$DOC    - This member.                                     *   FILE 090
//*                                                                 *   FILE 090
//*     $$DOC2   - Details of the March 2006 changes.               *   FILE 090
//*                                                                 *   FILE 090
//*     DCFDOC   - This is a documentation source file Dave made    *   FILE 090
//*                in SGML.  It can easily be converted to GML      *   FILE 090
//*                and run through SCRIPT/VS.  It can be TEXT       *   FILE 090
//*                transferred to DELINK.IPF on the PC for          *   FILE 090
//*                processing by the IPF compiler.  I resolved a    *   FILE 090
//*                lot (but not all) of character symbolics to      *   FILE 090
//*                get it to fit into an 80-column file.  It        *   FILE 090
//*                also made the uncompiled source more             *   FILE 090
//*                readable.                                        *   FILE 090
//*                                                                 *   FILE 090
//*                   &apos.             was replaced by   '        *   FILE 090
//*                   &asterisk.         was replaced by   *        *   FILE 090
//*                   &colon.            was replaced by   :        *   FILE 090
//*                   &comma.            was replaced by   ,        *   FILE 090
//*                   &eq.               was replaced by   =        *   FILE 090
//*                   &hyphen.           was replaced by   -        *   FILE 090
//*                   &lpar.             was replaced by   (        *   FILE 090
//*                   &per.              was replaced by   .        *   FILE 090
//*                   &plus.             was replaced by   +        *   FILE 090
//*                   &rpar.             was replaced by   )        *   FILE 090
//*                   &slash.            was replaced by   /        *   FILE 090
//*                                                                 *   FILE 090
//*     DELINK   - This is the main PL/I source member.             *   FILE 090
//*                It should be compiled with OS PL/I Version 2     *   FILE 090
//*                or with PL/I for MVS & VM.  Requires DWNSCAN     *   FILE 090
//*                and DWNSHEX to be linked into the program        *   FILE 090
//*                executable, and DWNSPDSR to be fetchable         *   FILE 090
//*                during execution.                                *   FILE 090
//*                                                                 *   FILE 090
//*     DWNMPRLG - PL/I prologue macro used by DWNSCAN and          *   FILE 090
//*                DWNSHEX.  Seems to work for OS PL/I Version 2    *   FILE 090
//*                and PL/I for MVS & VM.                           *   FILE 090
//*                                                                 *   FILE 090
//*     DWNSCAN  - Performs the same function as the PL/I SEARCH    *   FILE 090
//*                built-in function.  The SEARCH and SEARCHR       *   FILE 090
//*                built-in functions are not yet available under   *   FILE 090
//*                MVS (OS/390) at the time of writing.  This       *   FILE 090
//*                module should be assembled and made available    *   FILE 090
//*                at bind (ie. link-edit) time for inclusion       *   FILE 090
//*                into the main program.                           *   FILE 090
//*                                                                 *   FILE 090
//*     DWNSHEX  - Performs a similar function to the PL/I HEX      *   FILE 090
//*                built-in function.  The HEX and HEXIMAGE         *   FILE 090
//*                built-in functions are not yet available         *   FILE 090
//*                under MVS (OS/390) at the time of writing.       *   FILE 090
//*                This module should be assembled and made         *   FILE 090
//*                available at bind (ie. link-edit) time for       *   FILE 090
//*                inclusion into the main program.                 *   FILE 090
//*                                                                 *   FILE 090
//*     DWNSPDSR - Provides BPAM support for the main PL/I          *   FILE 090
//*                program.  It should be assembled and made        *   FILE 090
//*                available for dynamic fetching at execute        *   FILE 090
//*                time.                                            *   FILE 090
//*                                                                 *   FILE 090
//*     DWNYBLDL - PL/I source structure for PDS program            *   FILE 090
//*                directory entry.  It was obviously meant to      *   FILE 090
//*                be included in the source by some strange        *   FILE 090
//*                control card (not %INCLUDE), so I just copied    *   FILE 090
//*                it into the source.  This member is therefore    *   FILE 090
//*                no longer used.                                  *   FILE 090
//*                                                                 *   FILE 090
//*     FMBLOCK  - Housekeeping macros used by DWNSPDSR, all of     *   FILE 090
//*     FMCREDT    which were probably contributed to the SHARE     *   FILE 090
//*     FMSTART    tape by Ken True of Fairchild MSS (hence FM,     *   FILE 090
//*     FMWORK1    no doubt).  They were moved from from the        *   FILE 090
//*     FMWORK2    SHARE tape to the Fairchild MSS "Mods" tape,     *   FILE 090
//*                later called the Intel MVS "Mods" tape, circa    *   FILE 090
//*                1982.                                            *   FILE 090
//*                                                                 *   FILE 090
//*     PLIICB   - PL/I Interrupt Control Block macro used by       *   FILE 090
//*                DWNSHEX on error conditions.                     *   FILE 090
//*                                                                 *   FILE 090
//*     PLISIG   - PL/I Signal macro used by DWNSHEX on error       *   FILE 090
//*                conditions.                                      *   FILE 090
//*                                                                 *   FILE 090
//*     XMITBOOK - TSO/E transmit file of DELINK.INF which was      *   FILE 090
//*                created by compiling DELINK.IPF (source in       *   FILE 090
//*                member DCFDOC) with IPFC under OS/2.  Process    *   FILE 090
//*                with INDATASET operand of the TSO/E RECEIVE      *   FILE 090
//*                command to get a RECFM=U sequential data set.    *   FILE 090
//*                BINARY transfer this file to the PC (byte        *   FILE 090
//*                counts should match).  Use the VIEW command of   *   FILE 090
//*                OS/2 or PC-DOS to look at DELINK.INF which       *   FILE 090
//*                contains the Delinker documentation.  The        *   FILE 090
//*                PC-DOS VIEW command also works from the MS-DOS   *   FILE 090
//*                prompt (including under Win95).  Apparently,     *   FILE 090
//*                there is an IVIEW command downloadable from      *   FILE 090
//*                IBM designed to work under Win95.                *   FILE 090
//*                                                                 *   FILE 090
```
