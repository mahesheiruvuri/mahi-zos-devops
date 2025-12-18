# Copilot Instructions for mahi-zos-devops

This repository contains z/OS mainframe development samples with COBOL programs, copybooks, and JCL scripts.

## Architecture Overview

- **COBOL Programs**: Located in `COBOL/` directory (e.g., `SAMP1.cbl`). These are batch programs using standard COBOL syntax.
- **Copybooks**: Shared data definitions in `COPYBOOK/` (e.g., `STDREC.cpy`). Included in programs via `COPY` statement.
- **JCL Scripts**: Batch job control in `JCL/` (e.g., `rename.jcl`). Used for compilation, execution, and dataset management.

Data flows from copybooks to programs via COPY inclusion. JCL handles execution and file operations on z/OS.

## Key Patterns

- **Copybook Inclusion**: Use `COPY copybook-name.` in WORKING-STORAGE SECTION. Example from `COBOL/SAMP1.cbl`:
  ```
  WORKING-STORAGE SECTION.
  COPY STDREC.
  ```
  This includes `01 STDNAME PIC A(50) VALUE 'MAHESH'.` from `COPYBOOK/STDREC.cpy`.

- **COBOL Structure**: Follow standard divisions: IDENTIFICATION, ENVIRONMENT, DATA, PROCEDURE. Programs end with `STOP RUN.`

- **JCL Operations**: Use `//JOBNAME JOB` for job cards. Example dataset rename in `JCL/rename.jcl`:
  ```
  //COMPILE  EXEC PGM=IKJEFT01
  //SYSTSIN  DD *
    RENAME 'Z30120.TRAIN.COPYLIB' 'Z30120.TRAIN.COPY'
  ```

## Workflows

- **Compilation**: Use JCL with appropriate EXEC PGM (e.g., IGYCRCTL for COBOL compile).
- **Execution**: Submit JCL jobs to run programs.
- **Dataset Management**: Use TSO commands in JCL for file operations.

No local build system; development assumes z/OS environment. Test by submitting JCL to mainframe.

## Conventions

- File extensions: .cbl for COBOL, .cpy for copybooks, .jcl for JCL.
- Naming: Uppercase for program-ids, job names.
- Data types: PIC clauses for fields (e.g., PIC A(50) for alphanumeric).

Focus on mainframe-specific syntax and JCL scripting for productivity.