CMAKE_MINIMUM_REQUIRED (VERSION 3.11)

PROJECT (PTHREADS4W VERSION 3)
INCLUDE (GNUInstallDirs)
SET (CMAKE_DEBUG_POSTFIX d)

IF (EXISTS "${CMAKE_SOURCE_DIR}/.git")
	SET (DEFAULT_BUILD_TYPE "Debug")
ENDIF (EXISTS "${CMAKE_SOURCE_DIR}/.git")

# Set the possible values of build type for cmake-gui
IF (CMAKE_CONFIGURATION_TYPES)
	SET (CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE
		STRING "Semicolon separated list of supported configuration types, only supports debug and release, anything else will be ignored" FORCE
	)

	SET_PROPERTY (CACHE CMAKE_CONFIGURATION_TYPES PROPERTY STRINGS
		"Debug" "Release"
	)
ENDIF (CMAKE_CONFIGURATION_TYPES)

IF (DEFINED debug AND debug)
	SET (CMAKE_BUILD_TYPE "Debug" CACHE
		STRING "Choose the type of build." FORCE)
	MESSAGE (STATUS "Setting build type to Debug as specified.")
ELSEIF (DEFINED debug AND NOT debug)
	SET (CMAKE_BUILD_TYPE "Release" CACHE
		STRING "Choose the type of build." FORCE)
	MESSAGE (STATUS "Setting build type to Release as specified.")
ENDIF (DEFINED debug AND debug)

IF (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
	MESSAGE (STATUS "Setting build type to '${DEFAULT_BUILD_TYPE}' as none was specified.")
	SET (CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}" CACHE
		STRING "Choose the type of build." FORCE
	)
ENDIF (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)

#
# Check for DEBUG flag
#
IF (CMAKE_BUILD_TYPE STREQUAL "Debug")
	OPTION (debug "extra debugging code" ON)
ELSE (CMAKE_BUILD_TYPE STREQUAL "Debug")
	OPTION (debug "extra debugging code" OFF)
ENDIF (CMAKE_BUILD_TYPE STREQUAL "Debug")

SET (PTHREAD_SOURCES
	cleanup.c
	create.c
	dll.c
	errno.c
	global.c
	pthread_attr_destroy.c
	pthread_attr_getaffinity_np.c
	pthread_attr_getdetachstate.c
	pthread_attr_getinheritsched.c
	pthread_attr_getname_np.c
	pthread_attr_getschedparam.c
	pthread_attr_getschedpolicy.c
	pthread_attr_getscope.c
	pthread_attr_getstackaddr.c
	pthread_attr_getstacksize.c
	pthread_attr_init.c
	pthread_attr_setaffinity_np.c
	pthread_attr_setdetachstate.c
	pthread_attr_setinheritsched.c
	pthread_attr_setname_np.c
	pthread_attr_setschedparam.c
	pthread_attr_setschedpolicy.c
	pthread_attr_setscope.c
	pthread_attr_setstackaddr.c
	pthread_attr_setstacksize.c
	pthread_barrier_destroy.c
	pthread_barrier_init.c
	pthread_barrier_wait.c
	pthread_barrierattr_destroy.c
	pthread_barrierattr_getpshared.c
	pthread_barrierattr_init.c
	pthread_barrierattr_setpshared.c
	pthread_cancel.c
	pthread_cond_destroy.c
	pthread_cond_init.c
	pthread_cond_signal.c
	pthread_cond_wait.c
	pthread_condattr_destroy.c
	pthread_condattr_getpshared.c
	pthread_condattr_init.c
	pthread_condattr_setpshared.c
	pthread_delay_np.c
	pthread_detach.c
	pthread_equal.c
	pthread_exit.c
	pthread_getconcurrency.c
	pthread_getname_np.c
	pthread_getschedparam.c
	pthread_getspecific.c
	pthread_getunique_np.c
	pthread_getw32threadhandle_np.c
	pthread_join.c
	pthread_key_create.c
	pthread_key_delete.c
	pthread_kill.c
	pthread_mutex_consistent.c
	pthread_mutex_destroy.c
	pthread_mutex_init.c
	pthread_mutex_lock.c
	pthread_mutex_timedlock.c
	pthread_mutex_trylock.c
	pthread_mutex_unlock.c
	pthread_mutexattr_destroy.c
	pthread_mutexattr_getkind_np.c
	pthread_mutexattr_getpshared.c
	pthread_mutexattr_getrobust.c
	pthread_mutexattr_gettype.c
	pthread_mutexattr_init.c
	pthread_mutexattr_setkind_np.c
	pthread_mutexattr_setpshared.c
	pthread_mutexattr_setrobust.c
	pthread_mutexattr_settype.c
	pthread_num_processors_np.c
	pthread_once.c
	pthread_rwlock_destroy.c
	pthread_rwlock_init.c
	pthread_rwlock_rdlock.c
	pthread_rwlock_timedrdlock.c
	pthread_rwlock_timedwrlock.c
	pthread_rwlock_tryrdlock.c
	pthread_rwlock_trywrlock.c
	pthread_rwlock_unlock.c
	pthread_rwlock_wrlock.c
	pthread_rwlockattr_destroy.c
	pthread_rwlockattr_getpshared.c
	pthread_rwlockattr_init.c
	pthread_rwlockattr_setpshared.c
	pthread_self.c
	pthread_setaffinity.c
	pthread_setcancelstate.c
	pthread_setcanceltype.c
	pthread_setconcurrency.c
	pthread_setname_np.c
	pthread_setschedparam.c
	pthread_setspecific.c
	pthread_spin_destroy.c
	pthread_spin_init.c
	pthread_spin_lock.c
	pthread_spin_trylock.c
	pthread_spin_unlock.c
	pthread_testcancel.c
	pthread_timechange_handler_np.c
	pthread_timedjoin_np.c
	pthread_tryjoin_np.c
	pthread_win32_attach_detach_np.c
	ptw32_calloc.c
	ptw32_callUserDestroyRoutines.c
	ptw32_cond_check_need_init.c
	ptw32_getprocessors.c
	ptw32_is_attr.c
	ptw32_MCS_lock.c
	ptw32_mutex_check_need_init.c
	ptw32_new.c
	ptw32_processInitialize.c
	ptw32_processTerminate.c
	ptw32_relmillisecs.c
	ptw32_reuse.c
	ptw32_rwlock_cancelwrwait.c
	ptw32_rwlock_check_need_init.c
	ptw32_semwait.c
	ptw32_spinlock_check_need_init.c
	ptw32_threadDestroy.c
	ptw32_threadStart.c
	ptw32_throw.c
	ptw32_timespec.c
	ptw32_tkAssocCreate.c
	ptw32_tkAssocDestroy.c
	sched_get_priority_max.c
	sched_get_priority_min.c
	sched_getscheduler.c
	sched_setaffinity.c
	sched_setscheduler.c
	sched_yield.c
	sem_close.c
	sem_destroy.c
	sem_getvalue.c
	sem_init.c
	sem_open.c
	sem_post_multiple.c
	sem_post.c
	sem_timedwait.c
	sem_trywait.c
	sem_unlink.c
	sem_wait.c
	w32_CancelableWait.c
)

SET (PTHREAD_HEADERS
	_ptw32.h
	pthread.h
	sched.h
)

ADD_LIBRARY (pthreadVC SHARED
	${PTHREAD_SOURCES}
)


TARGET_INCLUDE_DIRECTORIES (pthreadVC
	PUBLIC $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/pthread>
	PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}
)

SET_TARGET_PROPERTIES (pthreadVC PROPERTIES
	DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX}
	OUTPUT_NAME pthreat
	EXPORT_NAME Threads::Threads
	VERSION ${PTHREADS4W_VERSION}
	SOVERSION ${PTHREADS4W_VERSION_MAJOR}
	PUBLIC_HEADER "${PTHREAD_HEADERS}"
)

TARGET_COMPILE_DEFINITIONS (pthreadVC
	PRIVATE __PTW32_BUILD_INLINED HAVE_CONFIG_H
)

INSTALL (TARGETS pthreadVC
	DESTINATION ${CMAKE_INSTALL_LIBDIR}
	EXPORT PTHREADS4WCONFIG
	LIBRARY
	DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/pthread
)

INSTALL (EXPORT PTHREADS4WCONFIG
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake
)