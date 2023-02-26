Singularity recipe files for PGGB https://github.com/pangenome/pggb

Build a native-arch, optimised docker container
```
git clone -b v0.5.3 --single-branch git@github.com:pangenome/pggb.git pggb-0.5.3
sed -i 's/-march=sandybridge/-march=native/g' pggb-0.5.3/Dockerfile
sed -i 's/Generic/Release/g' pggb-0.5.3/Dockerfile
docker build -t pggb:0.5.3 pggb-0.5.3/
```

test docker build
```
docker run -it -v ${PWD}/pggb-0.5.3/data/:/data pggb:0.5.3 /bin/bash -c "pggb -i /data/HLA/DRB1-3123.fa.gz -p 70 -s 3000 -G 2000 -n 10 -t 16 -v -V 'gi|568815561:#' -o /data/out -M -m"
```

Build Apptainer container
```
ml apptainer/1.1
unset APPTAINER_BINDPATH
sudo -E apptainer build pggb-0.5.3.sif Apptainer
ln -s pggb-0.5.3.sif pggb
```

test apptainer (bind workspace for testing)
```
export APPTAINER_BINDPATH=/workspace
./pggb -i ./pggb-0.5.3/data/HLA/DRB1-3123.fa.gz -p 70 -s 3000 -G 2000 -n 10 -t 16 -v -V 'gi|568815561:#' -o /data/out -M -m

```

Deploy apptainer container and symlink to target install location.

Cleanup
```
docker rmi pggb:0.5.3
rm -rf pggb-0.5.3/
```
