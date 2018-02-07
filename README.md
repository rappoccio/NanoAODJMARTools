# NanoAODJMARTools
Tools for using the NANOAOD postprocessing framework for JMAR. 


## Obtaining JMAR NANOAOD

You should first follow the directions outlined in the [NanoAODJMAR](https://github.com/cms-jet/NanoAODJMAR) repository. This repository assumes the NANOAOD files follow that structure. 

## JMAR NANOAOD Analysis : With CMSSW

First, set up a new fastjet and fastjet-contrib. If you are working on `cmslpc` you can use my fastjet at `/uscms_data/d2/rappocc/fastjet/bare/install_330`. Otherwise to make your own you need to:

```
wget http://fastjet.fr/repo/fastjet-3.3.0.tar.gz
tar -zxvf fastjet-3.3.0.tar.gz
cd fastjet-3.3.0
./configure  --enable-pyext   --prefix=/my/working/dir/for/fastjet
make -j 10
make install


wget http://fastjet.hepforge.org/contrib/downloads/fjcontrib-1.032.tar.gz
cd fjcontrib-1.032
./configure --fastjet-config=/my/working/dir/for/fastjet/bin/fastjet-config
make -j 10
make fragile-shared
make install
make fragile-shared-install
```

Now make a `CMSSW` working area and get this code:
```
cmsrel CMSSW_9_4_4
cd CMSSW_9_4_4/src
cmsenv
git clone https://github.com/cms-jet/NanoAODJMARTools.git PhysicsTools/NanoAODJMARTools
```


You can then use the following XML files to go into `$CMSSW_BASE/config/toolbox/$SCRAM_ARCH/tools/selected/`

```
cp $CMSSW_BASE/src/PhysicsTools/NanoAODJMARTools/xmlfiles/* $CMSSW_BASE/config/toolbox/$SCRAM_ARCH/tools/selected/
scram setup fastjet
scram setup fastjet-contrib
```


Compile and run:
```
scram b -j 10
cd PhysicsTools/NanoAODJMARTools/test
python postproc_softdrop.py
```




## JMAR NANOAOD Analysis : Without CMSSW 

Coming soon. 


## Technical details

This assumes you have [fastjet 3.3.0](http://fastjet.fr/repo/doxygen-3.3.0/), which implements the python front-end to fastjet. The `fastjet-contrib` packages do not yet have a python implementation, so this is implemented [here](https://github.com/cms-jet/NanoAODJMARTools/blob/master/src/Recluster.cc) and [here](https://github.com/cms-jet/NanoAODJMARTools/blob/master/interface/Recluster.h).

