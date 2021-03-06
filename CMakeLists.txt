set(CMAKE_USER_MAKE_RULES_OVERRIDE_CXX ${CMAKE_HOME_DIRECTORY}/cmake/cxx_flag_overrides.cmake)
project(hermes)
	cmake_minimum_required(VERSION 2.6)

	#------------------------------------------------------------------------------
	# Main configuration options.
	#~~~~~~~~~~~~~~~~~~~~~~~~~~~~

	### BEGIN DEFAULT CONFIGURATION ###

	# The values below are sane defaults to produce a build on all platforms,
	# if you want to override them, set them in CMake.vars

	# HermesCommon
		set(HERMES_COMMON_DEBUG     YES)
		set(HERMES_COMMON_RELEASE   YES)
		# Tests settings.
		set(HERMES_COMMON_WITH_TESTS YES)
		set(COLOR_LOGGING	    NO)

	# Hermes2D:
	set(WITH_H2D                        YES)
		# Release and debug versions.
		set(H2D_DEBUG               YES)
		set(H2D_RELEASE             YES)
		# Optional parts of the library.
		set(H2D_WITH_GLUT 					YES)
		set(H2D_WITH_VIEWER_GUI 		NO)
		
		# Tests settings.
		set(H2D_WITH_TEST_EXAMPLES  YES)
		set(H2D_WITH_TESTS          YES)

	# Reporting and logging:
		set(REPORT_WITH_LOGO        YES)  #logo will be shown
		set(REPORT_TO_FILE          NO)  #reports will be mirrored to a file
		set(REPORT_ALL              NO)   #do not report all events, just selected ones
		set(REPORT_WARNING          YES)  #warnings will be reported
		set(REPORT_INTR_WARNING     NO)   #warnings about itegration points will NOT be reported
		set(REPORT_INFO             YES)  #info will be reported
		set(REPORT_VERBOSE          NO)   #info details will not be reported
		set(REPORT_TRACE            NO)   #code execution tracing will not be reported
		set(REPORT_TIME             NO)   #time will not be measured and time measurement will not be reported
		#set(REPORT_DEBUG           NO)   #debug events will depend on version which is compiled

  # Installation path:
    set(TARGET_ROOT             /usr/local) #default installation path for the library will be /usr/local/lib,
                                            #for include files /usr/local/include
	#### Solvers ###
	# Standard UMFPACK
		# UMFPACK is chosen by default.
		set(WITH_UMFPACK YES)
		
	# SuperLU
		# First option enables sequential SuperLU, which should reside in SUPERLU_ROOT/lib
		# and SUPERLU_ROOT/include. If SUPERLU_ROOT is not provided, the library will be searched
		# for in system directories. If SUPERLU_MT is set to YES, the multithreaded version of
		# SuperLU will be used. In this case, there should be either superlu_mt_PTHREAD.a and/or
		# superlu_mt_OPENMP.a libraries present in the search path (the former will be chosen if
		# WITH_OPENMP is set to NO, the latter otherwise). There should also be a subdirectory
		# SLU_MT in the include directory since SuperLU_MT uses different versions of the same-named
		# header files as the serial library.
		set(WITH_SUPERLU            NO)
		set(SUPERLU_ROOT            /opt/SuperLU_4.0/)
		set(SUPERLU_MT              NO)

	# PETSc
		# If PETSc is enabled, PETSC_ROOT and PETSC_ARCH must be set as well;
		# if PETSc is disabled, PETSC_ROOT and PETSC_ARCH will be ignored.
		set(WITH_PETSC              NO)
		set(PETSC_ROOT              /opt/petsc/petsc-3.1-p7)
		set(PETSC_ARCH              petsc-arch-without-scalar-type)

	# MUMPS
		# Sequential version of MUMPS may be enabled by setting YES on the following line
		# and pointing to the package installation directory with MUMPS_ROOT. To enable the
		# parallel version, setting WITH_MPI to YES in addition to that should suffice. If link
		# problems arise, it may be that the automatically found SCALAPACK libraries (reported
		# during the CMake configuration phase) do not match those used when compiling MUMPS.
		# In this case, set SCALAPACK_LIBRARIES manually in CMake.vars and reconfigure Hermes.
		set(WITH_MUMPS              NO)
		# set(MUMPS_ROOT            root/dir/of/mumps/)
		# set(SCALAPACK_LIBRARIES   -lscalapack -lblacs)

	# Trilinos
		# Enable support for Trilinos solvers.
		set(WITH_TRILINOS           NO)
		
		# Experimental
		set(WITH_ZOLTAN             NO)

	### Mesh format ###
		set(WITH_EXODUSII           NO)
		set(WITH_HDF5               NO)

	### Others ###
	# Parallel execution.
		# (tells the linker to use parallel versions of the selected solvers, if available):
		#
		# Enable OpenMP for shared memory processing.
		#
		set(WITH_OPENMP             NO)

		# If MPI is enabled, the MPI library installed on the system should be found by
		# CMake automatically. If the found library doesn't match the one used to compile the
		# particular MPI-dependent package, the other two options should be used to specify it.
		#
		set(WITH_MPI                NO)
		# set(MPI_LIBRARIES         -lmpi)
		# set(MPI_INCLUDE_PATH      /usr/include/openmpi

	### END DEFAULT CONFIGURATION ###
		
	set(CMAKE_MODULE_PATH ${CMAKE_HOME_DIRECTORY}/cmake)
	set(HERMES_COMMON_DIR ${PROJECT_SOURCE_DIR})
	set(HERMES_COMMON_LIB  hermes_common)
		
	### end DEFAULT CONFIGURATION ###

	#ADDED BY COLMAN
	IF (APPLE)
			IF (NOT CMAKE_SYSTEM_PROCESSOR MATCHES powerpc)
					IF (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
							## It's Mac OS X.

							SET (CMAKE_BUILD_TYPE "Release" CACHE STRING "Release is default." FORCE)

							SET(CMAKE_OSX_ARCHITECTURES "i386" CACHE STRING
									"compiling for OSX only in i386 format. Library with the
									 necessary symbol '_glutSetOption' was only available
									 in /usr/lib/libglut.dylib, which happened to be a pure
									 i386 library. Maybe fixed later." FORCE)
							IF (${CMAKE_VERBOSE_MAKEFILE} MATCHES "TRUE")
									MESSAGE(STATUS)
									MESSAGE(STATUS "*******************************************************************************")
									MESSAGE(STATUS "*        SUMMARY OF USED VARIABLES -> Details in AllVariables.txt             *")
									MESSAGE(STATUS "*******************************************************************************")
									MESSAGE(STATUS "COMPILING FOR OSX, i386 platform")
									MESSAGE(STATUS "CMAKE_ROOT ........................... = ${CMAKE_ROOT}")
									MESSAGE(STATUS "CMAKE_HOME_DIRECTORY ................. = ${CMAKE_HOME_DIRECTORY}")
									MESSAGE(STATUS "CMAKE_VERSION ........................ = ${CMAKE_VERSION}")
									MESSAGE(STATUS "CMAKE_GENERATOR ...................... = ${CMAKE_GENERATOR}")
									MESSAGE(STATUS "CMAKE_MODULE_PATH .................... = ${CMAKE_MODULE_PATH}")
									MESSAGE(STATUS "CMAKE_HOST_SYSTEM .................... = ${CMAKE_HOST_SYSTEM}")
									MESSAGE(STATUS "CMAKE_HOST_SYSTEM_VERSION ............ = ${CMAKE_HOST_SYSTEM_VERSION}")
									MESSAGE(STATUS "CMAKE_HOST_SYSTEM_NAME ............... = ${CMAKE_HOST_SYSTEM_NAME}")
									MESSAGE(STATUS "CMAKE_HOST_SYSTEM_PROCESSOR .......... = ${CMAKE_HOST_SYSTEM_PROCESSOR}")
									MESSAGE(STATUS "CMAKE_HOST_UNIX ...................... = ${CMAKE_HOST_UNIX}")
									MESSAGE(STATUS "CMAKE_BUILD_TYPE ..................... = ${CMAKE_BUILD_TYPE}")
									MESSAGE(STATUS "CMAKE_C_COMPILER ..................... = ${CMAKE_C_COMPILER}")
									MESSAGE(STATUS "CMAKE_CXX_COMPILER ................... = ${CMAKE_CXX_COMPILER}")
							ENDIF()
					ENDIF ()
			ENDIF (NOT CMAKE_SYSTEM_PROCESSOR MATCHES powerpc)
			# Use modified search procedures for some libraries on MacOS.
			include(${CMAKE_HOME_DIRECTORY}/OSX.cmake)
	ENDIF (APPLE)
	# end ADDED BY COLMAN

	# Allow to override the default values in CMake.vars:
	include(CMake.vars OPTIONAL)

	macro (DETERMINE_HERMES_COMMON_RELEASE_DEBUG HXD)
		if (WITH_${HXD})
			if(MSVC)
					# In MSVC, Release/Debug is distinguished in a special way
					# (see hermes_common/cmake/BuildAndInstallScripts.cmake)
					set(HERMES_COMMON_DEBUG YES)
					set(HERMES_COMMON_LIB_DEBUG ${HERMES_COMMON_LIB}-debug)
					set(HERMES_COMMON_RELEASE YES)
					set(HERMES_COMMON_LIB_RELEASE ${HERMES_COMMON_LIB})
			else(MSVC)
				if(${HXD}_DEBUG)
					set(HERMES_COMMON_DEBUG YES)
					set(HERMES_COMMON_LIB_DEBUG  ${HERMES_COMMON_LIB}-debug)
				endif(${HXD}_DEBUG)

				if(${HXD}_RELEASE)
					set(HERMES_COMMON_RELEASE YES)
					set(HERMES_COMMON_LIB_RELEASE ${HERMES_COMMON_LIB})
				endif(${HXD}_RELEASE)
			endif(MSVC)
		endif (WITH_${HXD})
	endmacro(DETERMINE_HERMES_COMMON_RELEASE_DEBUG)

	# Enable debugging symbols for all files all the time:
	# set(CMAKE_CXX_FLAGS "-g ${CMAKE_CXX_FLAGS}")

	if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
			# Disable all warnings and turn on only important ones:
			set(CMAKE_CXX_FLAGS "-w ${CMAKE_CXX_FLAGS}")
			set(CMAKE_CXX_FLAGS "-Wuninitialized -Wvla -Wsign-compare ${CMAKE_CXX_FLAGS}")

			set(RELEASE_FLAGS "-DNDEBUG -O3")
			set(DEBUG_FLAGS   "-g")
	endif(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")

	# Enabling multiprocessor build on MSVC
	if(MSVC)
		SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")
	endif(MSVC)

	# This overrides CXX flags for MSVC
	if(MSVC)
			set(MSVC_DEFINES "/DWIN32 /D_WINDOWS /Dpopen=_popen /Dpclose=_pclose /D__value=_value /Dfinite=_finite /Dhypot=_hypot /Disatty=_isatty /Dfileno=_fileno /D_CRT_SECURE_NO_WARNINGS /DYY_NO_UNISTD_H /D_USE_MATH_DEFINES /DIMPLEMENT_C99 /wd4275 /wd4251")
			set(CMAKE_CXX_FLAGS_DEBUG_INIT          "/D_DEBUG /Od /Ob2 /MDd /Zi /RTC1 ${MSVC_DEFINES}")
			set(DEBUG_FLAGS ${DEBUG_FLAGS} "${CMAKE_CXX_FLAGS_DEBUG_INIT}")
			set(CMAKE_CXX_FLAGS_MINSIZEREL_INIT     "/O2 /Ob2 /MD ${MSVC_DEFINES}")
			set(CMAKE_CXX_FLAGS_RELEASE_INIT        "/DNDEBUG /O2 /Ob2 /MD ${MSVC_DEFINES}")
			set(RELEASE_FLAGS ${RELEASE_FLAGS} "${CMAKE_CXX_FLAGS_RELEASE_INIT}")
			set(CMAKE_CXX_FLAGS_RELWITHDEBINFO_INIT "/DNDEBUG /O2 /Ob2 /MD /Zi ${MSVC_DEFINES}")
	else(MSVC)
			set(CMAKE_CXX_FLAGS_DEBUG_INIT          "${CMAKE_CXX_FLAGS_DEBUG_INIT} -D_DEBUG")
	endif(MSVC)


	if(H2D_WITH_TESTS OR HERMES_COMMON_WITH_TESTS)
		enable_testing()
		add_custom_target(test COMMAND ctest)
	endif(H2D_WITH_TESTS OR HERMES_COMMON_WITH_TESTS)

	#------------------------------------------------------------------------------
	# Find necessary packages according to the settings above.
	#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

		find_package(PTHREAD REQUIRED)
		find_package(XSD REQUIRED)
		find_package(XERCES REQUIRED)
		
		# Solvers.
		if(WITH_UMFPACK)
			find_package(UMFPACK REQUIRED)
			include_directories(${UMFPACK_INCLUDE_DIRS})
		endif(WITH_UMFPACK)

		if(MSVC)
			if(WITH_PETSC OR WITH_MUMPS OR WITH_TRILINOS OR WITH_SUPERLU)
					# The search for CLAPACK and associated libraries will be performed
					# every time the project is configured in MSVC (TODO: Find out if
					# CLAPACK_LIBRARY, BLAS_LIBRARY, F2C_LIBRARY and CLAPACK_INCLUDE_DIRS
					# could be cached to improve subsequent configuration performance).
					find_package(CLAPACK REQUIRED)
					include_directories(${CLAPACK_INCLUDE_DIRS})
			endif(WITH_PETSC OR WITH_MUMPS OR WITH_TRILINOS OR WITH_SUPERLU)
		else(MSVC)
			# All third party solvers require BLAS and LAPACK (both found in the package LAPACK).
			# FindLAPACK is Fortran-only so Fortran must be enabled as well.
			if(WITH_UMFPACK OR WITH_PETSC OR WITH_MUMPS OR WITH_TRILINOS OR WITH_SUPERLU)
				if (NOT LAPACK_FOUND)
						enable_language(Fortran)
						find_package(LAPACK REQUIRED)
								
						# If no error occured, LAPACK library has been found. Save the path to
						# it to cache, so that it will not be searched for during next 'cmake .'
						set(LAPACK_LIBRARIES  ${LAPACK_LIBRARIES}
										CACHE STRING  "Path to LAPACK/BLAS libraries.")
						set(LAPACK_FOUND      YES
										CACHE STRING  "Have LAPACK/BLAS libraries been found?")
				endif (NOT LAPACK_FOUND)
				
				add_definitions(-DWITH_BLAS)	
			endif(WITH_UMFPACK OR WITH_PETSC OR WITH_MUMPS OR WITH_TRILINOS OR WITH_SUPERLU)
		endif(MSVC)

		if(WITH_PETSC)
			find_package(PETSC REQUIRED)
			# For some header files, PETSc uses the same name for both real and complex
			# versions (they only reside in different directories), so we can't globally
			# set the architecture-specific include directory here, but rather on a
			# per-target basis.
			include_directories(${PETSC_INCLUDE_DIRS})
			include_directories(${PETSC_CPLX_INCLUDE_DIRS})
			set(PETSC_LIBRARIES ${PETSC_CPLX_LIBRARIES})
		endif(WITH_PETSC)

		if(WITH_MUMPS)
			find_package(MUMPS REQUIRED)
			if(WITH_MPI)
				find_package(SCALAPACK REQUIRED) # parallel version of MUMPS needs ScaLapack+BLACS
			endif(WITH_MPI)
			include_directories(${MUMPS_INCLUDE_DIR})
			set(MUMPS_LIBRARIES ${MUMPS_CPLX_LIBRARIES})
			LIST(APPEND MUMPS_LIBRARIES ${MUMPS_REAL_LIBRARIES})
		endif(WITH_MUMPS)

		if(WITH_TRILINOS)
			find_package(TRILINOS REQUIRED)
			include_directories(${TRILINOS_INCLUDE_DIR})
		endif(WITH_TRILINOS)
		
		# Experimental
		if(WITH_ZOLTAN)
			find_package(ZOLTAN REQUIRED)
			include_directories(${ZOLTAN_INCLUDE_DIR})
		endif(WITH_ZOLTAN)

		if(WITH_SUPERLU)
			find_package(SUPERLU REQUIRED)
			include_directories(${SUPERLU_INCLUDE_DIR})
			if(SUPERLU_MT)
				add_definitions(-DSLU_MT)
			endif(SUPERLU_MT)
		endif(WITH_SUPERLU)

		# SMP.
		if(WITH_OPENMP)
			find_package(OpenMP REQUIRED)
			set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS})
			set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS})
		endif(WITH_OPENMP)

		if(NOT REPORT_TO_FILE)
			add_definitions(-DHERMES_REPORT_NO_FILE)
		endif(NOT REPORT_TO_FILE)

    if(WITH_EXODUSII)
      find_package(EXODUSII REQUIRED)
      include_directories(${EXODUSII_INCLUDE_DIR})
    endif(WITH_EXODUSII)

		# If using any package that requires MPI (e.g. parallel versions of MUMPS, PETSC).
		if(WITH_MPI)
			if(NOT MPI_LIBRARIES OR NOT MPI_INCLUDE_PATH) # If MPI was not defined by the user
				find_package(MPI REQUIRED)
			else(NOT MPI_LIBRARIES OR NOT MPI_INCLUDE_PATH)
				if(NOT EXISTS MPI_INCLUDE_PATH)
					message(FATAL_ERROR "MPI include directory ${MPI_INCLUDE_PATH} not found")
				endif(NOT EXISTS MPI_INCLUDE_PATH)
			endif(NOT MPI_LIBRARIES OR NOT MPI_INCLUDE_PATH)
			include_directories(${MPI_INCLUDE_PATH})
		endif(WITH_MPI)

	# Match debug or release version of all other libraries.
	DETERMINE_HERMES_COMMON_RELEASE_DEBUG(H2D)

	# Main CMakeLists.txt for each library are in the following directories.
	add_subdirectory(hermes_common)
		
	if(WITH_H2D)
		add_subdirectory(hermes2d)
	endif(WITH_H2D)


	#-----------------------------------------------------------------------------
	# Report.
	#~~~~~~~~
	message("\nConfiguration results")
	message("---------------------")
	message("Build with UMFPACK: ${WITH_UMFPACK}")
	message("Build with PETSC: ${WITH_PETSC}")
	message("Build with MUMPS: ${WITH_MUMPS}")
	message("Build with SUPERLU${MT}: ${WITH_SUPERLU}")
	message("Build with TRILINOS: ${WITH_TRILINOS}")
	message("Build with MPI: ${WITH_MPI}")
	message("Build with OPENMP: ${WITH_OPENMP}")
	message("Build with EXODUSII: ${WITH_EXODUSII}")
	if(HAVE_TEUCHOS_STACKTRACE)
			message("Print Teuchos stacktrace on segfault: YES")
	else(HAVE_TEUCHOS_STACKTRACE)
			message("Print Teuchos stacktrace on segfault: NO")
	endif(HAVE_TEUCHOS_STACKTRACE)
	
  message("Report with logo: ${REPORT_WITH_LOGO}")
	if(REPORT_ALL)
		message("Report all events: YES")
	else(REPORT_ALL)
		message("Report warnings: ${REPORT_WARNING}")
		message("Report integration warnings: ${REPORT_INTR_WARNING}")
		message("Report results/info: ${REPORT_INFO}")
		message("Report results/info details: ${REPORT_VERBOSE}")
		message("Report program execution: ${REPORT_TRACE}")
		message("Report internal time measurements: ${REPORT_TIME}")
	endif(REPORT_ALL)
	message("---------------------")
	message("Hermes common library:")
	message("\tBuild Debug version: ${HERMES_COMMON_DEBUG}")
	message("\tBuild Release version: ${HERMES_COMMON_RELEASE}")
	message("\tBuild with tests: ${HERMES_COMMON_WITH_TESTS}")

	message("Build Hermes2D: ${WITH_H2D}")
	if(WITH_H2D)
		message("\tBuild Hermes2D Debug version: ${H2D_DEBUG}")
		message("\tBuild Hermes2D Release version: ${H2D_RELEASE}")
	message("---------------------")
		message("\tBuild Hermes2D with tests: ${H2D_WITH_TESTS}")
		message("\tBuild Hermes2D with test examples: ${H2D_WITH_TEST_EXAMPLES}")
	message("---------------------")
		message("\tBuild Hermes2D with GLUT: ${H2D_WITH_GLUT}")
		message("\tBuild Hermes2D with VIEWER_GUI: ${H2D_WITH_VIEWER_GUI}")
	endif(WITH_H2D)
	message("\n")
