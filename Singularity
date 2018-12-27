Bootstrap: yum
OSVersion: 7
MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
Include: yum

%help

%setup

%files

%labels

%environment

%post
    echo "Installing Development Tools YUM group"
    yum -y groupinstall "Development Tools"
    echo "Installing OpenMPI into container..."
    yum -y install wget
    OPENMPI_VERSION=2.1.2
    OPENMPI_NAME=openmpi-${OPENMPI_VERSION}
    wget https://download.open-mpi.org/release/open-mpi/v2.1/${OPENMPI_NAME}.tar.gz
    tar -xvf ${OPENMPI_NAME}.tar.gz
    cd ${OPENMPI_NAME}
    ./configure --prefix=/usr/local
    make all install
    /usr/local/bin/mpicc examples/ring_c.c -o /usr/bin/mpi_ring
    cd /

#osu bench
    OSU_VERSION=5.5
    wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-${OSU_VERSION}.tar.gz
    tar -xvf osu-micro-benchmarks-${OSU_VERSION}.tar.gz
    cd osu-micro-benchmarks-${OSU_VERSION}
    ./configure --prefix=/usr/local CC=/usr/local/bin/mpicc CXX=/usr/local/bin/mpicxx
    make
    make install

%runscript
    /usr/local/libexec/osu-micro-benchmarks/mpi/pt2pt/osu_bw 
