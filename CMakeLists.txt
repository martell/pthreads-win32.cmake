
cmake_minimum_required(VERSION 2.8.10)

# Set package properties
project(pthreads-win32)

if(NOT PTHREADS_WIN32_SOURCE)
	set(PTHREADS_WIN32_SOURCE ${CMAKE_CURRENT_SOURCE_DIR}/pthreads-win32)
	if(NOT EXISTS "${PTHREADS_WIN32_SOURCE}/pthread.h")
		message(FATAL_ERROR "Couldn't find pthread.h in ${PTHREADS_WIN32_SOURCE} - you probably need to run 'git submodule update --init' to pull in the submodule.")
	endif()
endif()

if("${CMAKE_CURRENT_SOURCE_DIR}" STREQUAL "${CMAKE_SOURCE_DIR}")
	set(PTHREADS_EMBEDDED_BUILD NO)
	option(PTHREADS_BUILD_STATIC "Build a static library instead of DLL?" off)
	option(PTHREADS_BUILD_INLINED "Build all sources in one translation unit?" off)
else()
	set(PTHREADS_EMBEDDED_BUILD YES)
endif()

function(pthreads_message _msg)
	if(NOT PTHREADS_QUIET)
		message(STATUS ${_msg})
	endif()
endfunction()

set(API
	${PTHREADS_WIN32_SOURCE}/pthread.h
	${PTHREADS_WIN32_SOURCE}/sched.h
	${PTHREADS_WIN32_SOURCE}/semaphore.h)

source_group(APIPROJ FILES ${API})

set(SOURCES
	${PTHREADS_WIN32_SOURCE}/attr.c
	${PTHREADS_WIN32_SOURCE}/barrier.c
	${PTHREADS_WIN32_SOURCE}/cancel.c
	${PTHREADS_WIN32_SOURCE}/cleanup.c
	${PTHREADS_WIN32_SOURCE}/condvar.c
	${PTHREADS_WIN32_SOURCE}/create.c
	${PTHREADS_WIN32_SOURCE}/dll.c
	${PTHREADS_WIN32_SOURCE}/autostatic.c
	${PTHREADS_WIN32_SOURCE}/errno.c
	${PTHREADS_WIN32_SOURCE}/exit.c
	${PTHREADS_WIN32_SOURCE}/fork.c
	${PTHREADS_WIN32_SOURCE}/global.c
	${PTHREADS_WIN32_SOURCE}/misc.c
	${PTHREADS_WIN32_SOURCE}/mutex.c
	${PTHREADS_WIN32_SOURCE}/nonportable.c
	${PTHREADS_WIN32_SOURCE}/private.c
	${PTHREADS_WIN32_SOURCE}/rwlock.c
	${PTHREADS_WIN32_SOURCE}/sched.c
	${PTHREADS_WIN32_SOURCE}/semaphore.c
	${PTHREADS_WIN32_SOURCE}/signal.c
	${PTHREADS_WIN32_SOURCE}/spin.c
	${PTHREADS_WIN32_SOURCE}/sync.c
	${PTHREADS_WIN32_SOURCE}/tsd.c)
set(RESOURCES
	${PTHREADS_WIN32_SOURCE}/version.rc)
set(HEADERS
	${PTHREADS_WIN32_SOURCE}/config.h
	${PTHREADS_WIN32_SOURCE}/implement.h
	${PTHREADS_WIN32_SOURCE}/need_errno.h)

set(STATIC_SOURCES
	${PTHREADS_WIN32_SOURCE}/pthread_attr_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getdetachstate.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setdetachstate.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getstackaddr.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setstackaddr.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getstacksize.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setstacksize.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getscope.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setscope.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setschedpolicy.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getschedpolicy.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setschedparam.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getschedparam.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_setinheritsched.c
	${PTHREADS_WIN32_SOURCE}/pthread_attr_getinheritsched.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrier_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrier_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrier_wait.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrierattr_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrierattr_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrierattr_setpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_barrierattr_getpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_setcancelstate.c
	${PTHREADS_WIN32_SOURCE}/pthread_setcanceltype.c
	${PTHREADS_WIN32_SOURCE}/pthread_testcancel.c
	${PTHREADS_WIN32_SOURCE}/pthread_cancel.c
	${PTHREADS_WIN32_SOURCE}/cleanup.c
	${PTHREADS_WIN32_SOURCE}/pthread_condattr_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_condattr_getpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_condattr_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_condattr_setpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_cond_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_cond_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_cond_signal.c
	${PTHREADS_WIN32_SOURCE}/pthread_cond_wait.c
	${PTHREADS_WIN32_SOURCE}/create.c
	${PTHREADS_WIN32_SOURCE}/dll.c
	${PTHREADS_WIN32_SOURCE}/autostatic.c
	${PTHREADS_WIN32_SOURCE}/errno.c
	${PTHREADS_WIN32_SOURCE}/pthread_exit.c
	${PTHREADS_WIN32_SOURCE}/fork.c
	${PTHREADS_WIN32_SOURCE}/global.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_getpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_setpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_settype.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_gettype.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_setrobust.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_getrobust.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_lock.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_timedlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_unlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_trylock.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutex_consistent.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_setkind_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_mutexattr_getkind_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_getw32threadhandle_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_getunique_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_delay_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_num_processors_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_win32_attach_detach_np.c
	${PTHREADS_WIN32_SOURCE}/pthread_equal.c
	${PTHREADS_WIN32_SOURCE}/pthread_getconcurrency.c
	${PTHREADS_WIN32_SOURCE}/pthread_once.c
	${PTHREADS_WIN32_SOURCE}/pthread_self.c
	${PTHREADS_WIN32_SOURCE}/pthread_setconcurrency.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlockattr_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlockattr_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlockattr_getpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlockattr_setpshared.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_rdlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_wrlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_unlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_tryrdlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_rwlock_trywrlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_setschedparam.c
	${PTHREADS_WIN32_SOURCE}/pthread_getschedparam.c
	${PTHREADS_WIN32_SOURCE}/pthread_timechange_handler_np.c
	${PTHREADS_WIN32_SOURCE}/ptw32_is_attr.c
	${PTHREADS_WIN32_SOURCE}/ptw32_processInitialize.c
	${PTHREADS_WIN32_SOURCE}/ptw32_processTerminate.c
	${PTHREADS_WIN32_SOURCE}/ptw32_threadStart.c
	${PTHREADS_WIN32_SOURCE}/ptw32_threadDestroy.c
	${PTHREADS_WIN32_SOURCE}/ptw32_tkAssocCreate.c
	${PTHREADS_WIN32_SOURCE}/ptw32_tkAssocDestroy.c
	${PTHREADS_WIN32_SOURCE}/ptw32_callUserDestroyRoutines.c
	${PTHREADS_WIN32_SOURCE}/ptw32_timespec.c
	${PTHREADS_WIN32_SOURCE}/ptw32_throw.c
	${PTHREADS_WIN32_SOURCE}/ptw32_getprocessors.c
	${PTHREADS_WIN32_SOURCE}/ptw32_calloc.c
	${PTHREADS_WIN32_SOURCE}/ptw32_new.c
	${PTHREADS_WIN32_SOURCE}/ptw32_reuse.c
	${PTHREADS_WIN32_SOURCE}/ptw32_rwlock_check_need_init.c
	${PTHREADS_WIN32_SOURCE}/ptw32_cond_check_need_init.c
	${PTHREADS_WIN32_SOURCE}/ptw32_mutex_check_need_init.c
	${PTHREADS_WIN32_SOURCE}/ptw32_semwait.c
	${PTHREADS_WIN32_SOURCE}/ptw32_relmillisecs.c
	${PTHREADS_WIN32_SOURCE}/ptw32_MCS_lock.c
	${PTHREADS_WIN32_SOURCE}/sched_get_priority_max.c
	${PTHREADS_WIN32_SOURCE}/sched_get_priority_min.c
	${PTHREADS_WIN32_SOURCE}/sched_setscheduler.c
	${PTHREADS_WIN32_SOURCE}/sched_getscheduler.c
	${PTHREADS_WIN32_SOURCE}/sched_yield.c
	${PTHREADS_WIN32_SOURCE}/sem_init.c
	${PTHREADS_WIN32_SOURCE}/sem_destroy.c
	${PTHREADS_WIN32_SOURCE}/sem_trywait.c
	${PTHREADS_WIN32_SOURCE}/sem_timedwait.c
	${PTHREADS_WIN32_SOURCE}/sem_wait.c
	${PTHREADS_WIN32_SOURCE}/sem_post.c
	${PTHREADS_WIN32_SOURCE}/sem_post_multiple.c
	${PTHREADS_WIN32_SOURCE}/sem_getvalue.c
	${PTHREADS_WIN32_SOURCE}/sem_open.c
	${PTHREADS_WIN32_SOURCE}/sem_close.c
	${PTHREADS_WIN32_SOURCE}/sem_unlink.c
	${PTHREADS_WIN32_SOURCE}/signal.c
	${PTHREADS_WIN32_SOURCE}/pthread_kill.c
	${PTHREADS_WIN32_SOURCE}/ptw32_spinlock_check_need_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_spin_init.c
	${PTHREADS_WIN32_SOURCE}/pthread_spin_destroy.c
	${PTHREADS_WIN32_SOURCE}/pthread_spin_lock.c
	${PTHREADS_WIN32_SOURCE}/pthread_spin_unlock.c
	${PTHREADS_WIN32_SOURCE}/pthread_spin_trylock.c
	${PTHREADS_WIN32_SOURCE}/pthread_detach.c
	${PTHREADS_WIN32_SOURCE}/pthread_join.c
	${PTHREADS_WIN32_SOURCE}/pthread_key_create.c
	${PTHREADS_WIN32_SOURCE}/pthread_key_delete.c
	${PTHREADS_WIN32_SOURCE}/pthread_setspecific.c
	${PTHREADS_WIN32_SOURCE}/pthread_getspecific.c
	${PTHREADS_WIN32_SOURCE}/w32_CancelableWait.c)


if(PTHREADS_BUILD_STATIC)
	set(SOURCES ${STATIC_SOURCES})
	set(PTHREADS_LIBTYPE STATIC)
else()
	set(PTHREADS_LIBTYPE SHARED)
endif()

if(PTHREADS_BUILD_INLINED)
	set(SOURCES ${PTHREADS_WIN32_SOURCE}/pthread.c)
	add_definitions(/DPTW32_BUILD_INLINED)
endif()

if( CMAKE_SIZEOF_VOID_P EQUAL 8 )
	add_definitions( -DPTW32_ARCH="x64" )
else( CMAKE_SIZEOF_VOID_P EQUAL 8 )
	add_definitions( -DPTW32_ARCH="x86" )
endif( CMAKE_SIZEOF_VOID_P EQUAL 8 )


if(MSVC)
	add_definitions(/DPTW32_RC_MSC)
	set(PTHREADS_COMPILER V)
elseif(CMAKE_COMPILER_IS_GNUCXX)
	set(PTHREADS_COMPILER G)
else()
	pthreads_message("WARNING: pthreads-win32 doesn't recognize your compiler!")
endif()

if(PTHREADS_BUILD_CPP)
	set(PTHREADS_EXCEPTION_SCHEME CE)
	add_definitions(/__CLEANUP_CXX)
	pthreads_message("Building pthreads-win32 with C++ exception handling.")
elseif(PTHREADS_BUILD_SEH)
	set(PTHREADS_EXCEPTION_SCHEME SE)
	add_definitions(/__CLEANUP_SEH)
	pthreads_message("Building pthreads-win32 with Microsoft SEH exception handling.")
else()
	# defaults to C - setjmp/longjmp
	set(PTHREADS_EXCEPTION_SCHEME C)
	pthreads_message("Building pthreads-win32 with C setjmp/longjmp (default/recommended exception scheme).")
endif()
set(PTHREADS_COMPATIBILITY_VERSION 2)
set(PTHREADS_LIBRARY "pthreads${PTHREADS_COMPILER}${PTHREADS_EXCEPTION_SCHEME}${PTHREADS_COMPATIBILITY_VERSION}")
pthreads_message("Library name of pthreads-win32: ${PTHREADS_LIBRARY}.")
set(CMAKE_DEBUG_POSTFIX d)

add_definitions(/DHAVE_PTW32_CONFIG_H)

include_directories(${PTHREADS_WIN32_SOURCE})
add_library(${PTHREADS_LIBRARY} ${PTHREADS_LIBTYPE} ${SOURCES} ${APIPROJ} ${RESOURCES})

set_target_properties(${PTHREADS_LIBRARY} PROPERTIES PUBLIC_HEADER ${API})

if(PTHREADS_EMBEDDED_BUILD)
	set(PTHREADS_LIBRARY ${PTHREADS_LIBRARY} PARENT_SCOPE)
	set(PTHREADS_INCLUDE_DIR ${PTHREADS_WIN32_SOURCE} PARENT_SCOPE)
else()
	set(PTHREADS_INSTALL YES)
	set(PTHREADS_BINDIR bin)
	set(PTHREADS_BINCOMPONENT runtime)
	set(PTHREADS_LIBDIR lib)
	set(PTHREADS_LIBCOMPONENT dev)
	set(PTHREADS_INCLUDEDIR include)
	set(PTHREADS_INCLUDECOMPONENT dev)
endif()

if(PTHREADS_INSTALL)
	install(TARGETS ${PTHREADS_LIBRARY}
		RUNTIME DESTINATION ${PTHREADS_BINDIR} COMPONENT ${PTHREADS_BINCOMPONENT}
		LIBRARY DESTINATION ${PTHREADS_LIBDIR} COMPONENT ${PTHREADS_LIBCOMPONENT}
		ARCHIVE DESTINATION ${PTHREADS_LIBDIR} COMPONENT ${PTHREADS_LIBCOMPONENT}
		PUBLIC_HEADER DESTINATION ${PTHREADS_INCLUDEDIR} COMPONENT ${PTHREADS_INCLUDECOMPONENT})
endif()