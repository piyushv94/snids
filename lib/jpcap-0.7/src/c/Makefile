# Makefile for jpcap
# Copyright (c) 2000 Keita Fujii

# You should set JAVA_HOME before running 'make'
# Or, you can manually change the following line, e.g., 'JAVA_DIR = /usr/java/latest'
JAVA_DIR = $(JAVA_HOME)

JNI_INCLUDE = $(JAVA_DIR)/include

# try to detect your OS using 'uname'
PLATFORM = $(shell "uname")

ifeq ($(PLATFORM), Linux)
    JNI_INCLUDE2 = $(JAVA_DIR)/include/linux
    COMPILE_OPTION = -fPIC -shared -L.
    SUFFIX = .so
else
ifeq ($(PLATFORM), FreeBSD)
    JNI_INCLUDE2 = $(JAVA_DIR)/include/freebsd
    COMPILE_OPTION = -fPIC -shared -L.
    SUFFIX = .so
else
ifeq ($(PLATFORM), SunOS)
    JNI_INCLUDE2 = $(JAVA_DIR)/include/solaris
    COMPILE_OPTION = -G
    SUFFIX = .so
else
ifeq ($(PLATFORM), Darwin)
    JNI_INCLUDE2 = /System/Library/Frameworks/JavaVM.framework/Headers 
    COMPILE_OPTION = -bundle -framework JavaVM
    SUFFIX = .jnilib
endif
endif
endif
endif

# Specify where pcap.h is
PCAP_INCLUDE = /usr/include
#PCAP_INCLUDE = /usr/include/pcap

#CC = cl
CC = gcc

all: libjpcap.so

libjpcap.so: JpcapCaptor.c JpcapSender.c JpcapWriter.c\
             packet_arp.c packet_datalink.c packet_icmp.c packet_ip.c\
             packet_ipv6.c packet_tcp.c packet_udp.c 
		$(CC) $(COMPILE_OPTION) -I$(JNI_INCLUDE) -I$(JNI_INCLUDE2)\
		 -I$(PCAP_INCLUDE)\
		JpcapCaptor.c JpcapSender.c JpcapWriter.c\
          packet_arp.c packet_datalink.c packet_icmp.c packet_ip.c\
          packet_ipv6.c packet_tcp.c packet_udp.c\
		-o libjpcap$(SUFFIX) -lpcap
clean:
		-rm libjpcap$(SUFFIX)
