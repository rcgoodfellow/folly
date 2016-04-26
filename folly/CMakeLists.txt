cmake_minimum_required(VERSION 3.0)
project(folly)

include_directories("${folly_SOURCE_DIR}/..")

add_subdirectory(3p/glog)
#add_subdirectory(3p/gflags)
add_subdirectory(3p/double-conversion)

set(CMAKE_CXX_COMPILER clang++)
set(CMAKE_C_COMPILER clang)

set(CMAKE_CXX_FLAGS "-std=c++14 -fcolor-diagnostics")

add_library(follybase
	Conv.cpp 
	Demangle.cpp 
	detail/RangeCommon.cpp 
	detail/RangeSse42.cpp 
	EscapeTables.cpp 
	Format.cpp 
	FormatTables.cpp 
	StringBase.cpp 
	String.cpp 
	Unicode.cpp
)

add_executable(generate_fingerprint_tables build/GenerateFingerprintTables.cpp)
target_link_libraries(generate_fingerprint_tables follybase gflags glog double-conversion)

add_custom_command(OUTPUT FingerprintTables.cpp
  COMMAND ${folly_BINARY_DIR}/generate_fingerprint_tables
  DEPENDS generate_fingerprint_tables)

execute_process(COMMAND python ${folly_SOURCE_DIR}/build/generate_format_tables.py)
execute_process(COMMAND mv ${folly_BINARY_DIR}/FormatTables.cpp ${folly_SOURCE_DIR}/)

execute_process(COMMAND python ${folly_SOURCE_DIR}/build/generate_escape_tables.py)
execute_process(COMMAND mv ${folly_BINARY_DIR}/EscapeTables.cpp ${folly_SOURCE_DIR}/)

execute_process(COMMAND python ${folly_SOURCE_DIR}/build/generate_varint_tables.py)
execute_process(COMMAND mv ${folly_BINARY_DIR}/GroupVarintTables.cpp ${folly_SOURCE_DIR}/)

add_library(folly
	Bits.cpp 
	Checksum.cpp 
	detail/CacheLocality.cpp 
	dynamic.cpp 
	File.cpp 
	FileUtil.cpp 
	FingerprintTables.cpp 
	futures/Barrier.cpp 
	futures/ThreadedExecutor.cpp 
	futures/Future.cpp 
	futures/InlineExecutor.cpp 
	futures/ManualExecutor.cpp 
	futures/QueuedImmediateExecutor.cpp 
	futures/ThreadWheelTimekeeper.cpp 
	detail/Futex.cpp 
	detail/StaticSingletonManager.cpp 
	detail/ThreadLocalDetail.cpp 
	GroupVarint.cpp 
	GroupVarintTables.cpp 
	IPAddress.cpp 
	IPAddressV4.cpp 
	IPAddressV6.cpp 
	LifoSem.cpp 
	io/Compression.cpp 
	io/IOBuf.cpp 
	io/IOBufQueue.cpp 
	io/RecordIO.cpp 
	io/ShutdownSocketSet.cpp 
	io/async/AsyncPipe.cpp 
	io/async/AsyncTimeout.cpp 
	io/async/AsyncUDPSocket.cpp 
	io/async/AsyncServerSocket.cpp 
	io/async/AsyncSignalHandler.cpp 
	io/async/AsyncSocket.cpp 
	io/async/AsyncSSLSocket.cpp 
	io/async/EventBase.cpp 
	io/async/EventBaseLocal.cpp 
	io/async/EventBaseManager.cpp 
	io/async/EventHandler.cpp 
	io/async/Request.cpp 
	io/async/SSLContext.cpp 
	io/async/ScopedEventBaseThread.cpp 
	io/async/HHWheelTimer.cpp 
	io/async/test/SocketPair.cpp 
	io/async/test/TimeUtil.cpp 
	io/async/ssl/OpenSSLUtils.cpp 
	json.cpp 
	detail/MemoryIdler.cpp 
	MacAddress.cpp 
	MemoryMapping.cpp 
	portability/Dirent.cpp 
	portability/Environment.cpp 
	portability/Libgen.cpp 
	portability/Malloc.cpp 
	portability/Memory.cpp 
	portability/String.cpp 
	portability/SysFile.cpp 
	portability/SysMman.cpp 
	portability/SysResource.cpp 
	portability/SysStat.cpp 
	portability/SysTime.cpp 
	portability/Time.cpp 
	Random.cpp 
	SafeAssert.cpp 
	SharedMutex.cpp 
	MicroLock.cpp 
	Singleton.cpp 
	SocketAddress.cpp 
	SpookyHashV1.cpp 
	SpookyHashV2.cpp 
	stats/Instantiations.cpp 
	Subprocess.cpp 
	ThreadCachedArena.cpp 
	TimeoutQueue.cpp 
	Uri.cpp 
	Version.cpp 
	experimental/bser/Dump.cpp 
	experimental/bser/Load.cpp 
	experimental/DynamicParser.cpp 
	experimental/fibers/Baton.cpp 
	experimental/fibers/Fiber.cpp 
	experimental/fibers/FiberManager.cpp 
	experimental/fibers/FiberManagerMap.cpp 
	experimental/fibers/GuardPageAllocator.cpp 
	experimental/fibers/TimeoutController.cpp 
	experimental/FunctionScheduler.cpp 
	experimental/io/FsUtil.cpp 
	experimental/JSONSchema.cpp 
	experimental/NestedCommandLineApp.cpp 
	experimental/ProgramOptions.cpp 
	experimental/Select64.cpp 
	experimental/TestUtil.cpp
)
