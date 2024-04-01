ifeq ($(NERSC_HOST),perlmutter)
 # The Cray environment on Perlmutter uses "ftn" and "cc" as the wrappers for MPI
    FC := ftn
    CC := cc
else
    FC := mpif90
    CC := gcc
endif

# Get the actual Fortran compiler invoked by mpif90. Need to use double $$ in awk.
BASE_FCOMP := $(shell mpif90 -show | awk '{print $$1}' | awk -F/ '{print $$NF}' | awk -F- '{print $$1}')

$(info ${BASE_FCOMP} )

# Git version
GIT_HASH=$(shell git rev-parse HEAD)
GIT_BRANCH=$(shell git branch | grep "^\*" | sed 's/^..//')
GIT_DATE=$(shell git log -1 --format=%cd)


ifeq ($(BASE_FCOMP),ifort)
   $(info "Using Intel compiler" )
else ifeq ($(BASE_FCOMP),gfortran)
   $(info "Using gcc/gfortran compilers" )
   FFLAGS := -ffree-line-length-none -fallow-argument-mismatch
endif

CFLAGS := 
FFLAGS += 


OBJ := WELL19937a_new.o \
       parRandomNumberInterface.o \
       parModules.o \
       parBeamInPlasma.o \
       parBeamInjection.o \
       parCheckpoint.o \
       parCollisionProc.o \
       parCurProbVals.o \
       parDiagNoGraph.o \
       parFieldCalcProc.o \
       parIonInducedSEEmission.o \
       parLangevinColl.o \
       parMCCollisions.o \
       parMainProgram.o \
       parPartDynamicProc.o \
       parPartExchangeProc.o \
       parPlasmaSourceProc.o \
       parSEEmission.o \
       parSnapshot.o

edipic1d : version_info $(OBJ)
	$(FC) $(FFLAGS) -o $@ $(OBJ) $(LIBS)

%.o : %.f90
	$(FC) $(FFLAGS) -c $<

%.o : %.F90
	$(FC) $(FFLAGS) -c $<

%.o : %.c
	$(CC) $(CFLAGS) -c $<

clean:
	rm -f *.mod *.o edipic1d

version_info:
	@echo ""
	@echo "     Branch: $(GIT_BRANCH)"
	@echo "     Commit ID: $(GIT_HASH)"
	@echo "     Commit date: $(GIT_DATE)"
	@echo ""
