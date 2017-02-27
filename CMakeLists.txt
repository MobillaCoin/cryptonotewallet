cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB americancoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    americancoin/external
    americancoin/include
    americancoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("americancoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  americancoin/external/miniupnpc/connecthostport.c
  americancoin/external/miniupnpc/igd_desc_parse.c
  americancoin/external/miniupnpc/minisoap.c
  americancoin/external/miniupnpc/miniupnpc.c
  americancoin/external/miniupnpc/miniwget.c
  americancoin/external/miniupnpc/minixml.c
  americancoin/external/miniupnpc/portlistingparse.c
  americancoin/external/miniupnpc/receivedata.c
  americancoin/external/miniupnpc/upnpcommands.c
  americancoin/external/miniupnpc/upnpreplyparse.c
  americancoin/src/Common/Base58.cpp
  americancoin/src/Common/CommandLine.cpp
  americancoin/src/Common/Util.cpp
  americancoin/src/Common/StringTools.cpp
  americancoin/src/Common/JsonValue.cpp
  americancoin/src/Common/ConsoleTools.cpp
  americancoin/src/Common/MemoryInputStream.cpp
  americancoin/src/Common/PathTools.cpp
  americancoin/src/Common/StdInputStream.cpp
  americancoin/src/Common/StdOutputStream.cpp
  americancoin/src/Common/StreamTools.cpp
  americancoin/src/Common/StringOutputStream.cpp
  americancoin/src/Common/StringView.cpp
  americancoin/src/Common/VectorOutputStream.cpp
  americancoin/src/crypto/blake256.c
  americancoin/src/crypto/chacha8.c
  americancoin/src/crypto/crypto-ops-data.c
  americancoin/src/crypto/crypto-ops.c
  americancoin/src/crypto/crypto.cpp
  americancoin/src/crypto/groestl.c
  americancoin/src/crypto/hash-extra-blake.c
  americancoin/src/crypto/hash-extra-groestl.c
  americancoin/src/crypto/hash-extra-jh.c
  americancoin/src/crypto/hash-extra-skein.c
  americancoin/src/crypto/hash.c
  americancoin/src/crypto/jh.c
  americancoin/src/crypto/keccak.c
  americancoin/src/crypto/oaes_lib.c
  americancoin/src/crypto/random.c
  americancoin/src/crypto/skein.c
  americancoin/src/crypto/slow-hash.c
  americancoin/src/crypto/slow-hash.cpp
  americancoin/src/crypto/tree-hash.c
  americancoin/src/CryptoNoteCore/BlockchainIndices.cpp
  americancoin/src/CryptoNoteCore/BlockchainMessages.cpp
  americancoin/src/CryptoNoteCore/BlockIndex.cpp
  americancoin/src/CryptoNoteCore/CoreConfig.cpp
  americancoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  americancoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  americancoin/src/CryptoNoteCore/Currency.cpp
  americancoin/src/CryptoNoteCore/MinerConfig.cpp
  americancoin/src/CryptoNoteCore/Transaction.cpp
  americancoin/src/CryptoNoteCore/Account.cpp
  americancoin/src/CryptoNoteCore/Blockchain.cpp
  americancoin/src/CryptoNoteCore/Checkpoints.cpp
  americancoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  americancoin/src/CryptoNoteCore/Core.cpp
  americancoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  americancoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  americancoin/src/CryptoNoteCore/Difficulty.cpp
  americancoin/src/CryptoNoteCore/IBlock.cpp
  americancoin/src/CryptoNoteCore/Miner.cpp
  americancoin/src/CryptoNoteCore/TransactionExtra.cpp
  americancoin/src/CryptoNoteCore/TransactionPool.cpp
  americancoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  americancoin/src/CryptoNoteCore/TransactionUtils.cpp
  americancoin/src/InProcessNode/InProcessNode.cpp
  americancoin/src/InProcessNode/InProcessNodeErrors.cpp
  americancoin/src/NodeRpcProxy/NodeErrors.cpp
  americancoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  americancoin/src/P2p/NetNodeConfig.cpp
  americancoin/src/Serialization/BinaryInputStreamSerializer.cpp
  americancoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  americancoin/src/Serialization/JsonInputValueSerializer.cpp
  americancoin/src/Serialization/JsonOutputStreamSerializer.cpp
  americancoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  americancoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  americancoin/src/Serialization/SerializationOverloads.cpp
  americancoin/src/Transfers/BlockchainSynchronizer.cpp
  americancoin/src/Transfers/SynchronizationState.cpp
  americancoin/src/Transfers/TransfersConsumer.cpp
  americancoin/src/Transfers/TransfersContainer.cpp
  americancoin/src/Transfers/TransfersSubscription.cpp
  americancoin/src/Transfers/TransfersSynchronizer.cpp
  americancoin/src/Wallet/LegacyKeysImporter.cpp
  americancoin/src/Wallet/WalletAsyncContextCounter.cpp
  americancoin/src/Wallet/WalletErrors.cpp
  americancoin/src/WalletLegacy/KeysStorage.cpp
  americancoin/src/WalletLegacy/WalletLegacy.cpp
  americancoin/src/WalletLegacy/WalletHelper.cpp
  americancoin/src/WalletLegacy/WalletLegacySerializer.cpp
  americancoin/src/WalletLegacy/WalletLegacySerialization.cpp
  americancoin/src/WalletLegacy/WalletTransactionSender.cpp
  americancoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  americancoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  americancoin/src/System/ContextGroup.cpp
  americancoin/src/System/Event.cpp
  americancoin/src/System/EventLock.cpp
  americancoin/src/System/InterruptedException.cpp
  americancoin/src/System/Ipv4Address.cpp
  americancoin/src/System/TcpStream.cpp
  americancoin/src/HTTP/HttpRequest.cpp
  americancoin/src/HTTP/HttpParser.cpp
  americancoin/src/HTTP/HttpParserErrorCodes.cpp
  americancoin/src/HTTP/HttpResponse.cpp
  americancoin/src/Rpc/HttpClient.cpp
  americancoin/src/Rpc/JsonRpc.cpp
  americancoin/src/P2p/NetNode.cpp
  americancoin/src/P2p/LevinProtocol.cpp
  americancoin/src/P2p/NetNodeConfig.cpp
  americancoin/src/P2p/PeerListManager.cpp
  americancoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  americancoin/src/Logging/ILogger.cpp
  americancoin/src/Logging/LoggerGroup.cpp
  americancoin/src/Logging/CommonLogger.cpp
  americancoin/src/Logging/LoggerManager.cpp
  americancoin/src/Logging/FileLogger.cpp
  americancoin/src/Logging/StreamLogger.cpp
  americancoin/src/Logging/ConsoleLogger.cpp
  americancoin/src/Logging/LoggerMessage.cpp
  americancoin/src/Logging/LoggerRef.cpp
  americancoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  americancoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  americancoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(americancoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/americancoinwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} americancoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE americancoin.icns)
    set(APPLICATION_ICON src/images/americancoin.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})

    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(americancoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES americancoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
