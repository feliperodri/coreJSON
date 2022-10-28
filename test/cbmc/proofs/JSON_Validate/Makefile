# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0

HARNESS_FILE=JSON_Validate_harness
PROOF_UID=JSON_Validate

# This value was experimentally chosen to provide 100% coverage
# without tripping unwinding assertions and without exhausting memory.
CBMC_MAX_BUFSIZE=14

UNWINDSET += JSON_Validate.0:$(CBMC_MAX_BUFSIZE)

#PROOF_SOURCES += $(PROOF_STUB)/skipGeneric.c
#PROOF_SOURCES += $(PROOF_STUB)/skipAnyLiteral.c
#PROOF_SOURCES += $(PROOF_STUB)/skipCollection.c
#PROOF_SOURCES += $(PROOF_STUB)/skipNumber.c
#PROOF_SOURCES += $(PROOF_STUB)/skipSpace.c
#PROOF_SOURCES += $(PROOF_STUB)/skipString.c

UNWINDSET += __CPROVER_file_local_core_json_c_countHighBits.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipArrayScalars.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipCollection.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipDigits.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipObjectScalars.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipOneHexEscape.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipSpace.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_skipString.0:$(CBMC_MAX_BUFSIZE)
UNWINDSET += __CPROVER_file_local_core_json_c_strnEq.0:$(CBMC_MAX_BUFSIZE)

include ../Makefile-json.common

# Substitution command to pass to sed for patching core_json.c. The
# characters " and # must be escaped with backslash.
#CORE_JSON_SED_EXPR = 1s/^/\#include \"core_json_annex.h\" /; s/^static //; s/(bool|JSONStatus_t|void) skip(AnyLiteral|Collection|Number|Space|String)\b/&_/
