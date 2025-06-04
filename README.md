# porouswavefoam
#It can generate stable wind fields and couple porous medium models, while allowing for the free definition of horizontal wave-overrun monitoring surfaces at different spatial locations behind the embankment, as well as pressure measurement points on the ground behind the embankment.
blockMesh

topoSet  
       
 subsetMesh obstacle  -overwrite -patch bottom
      subsetMesh obstacle1  -overwrite -patch bottom
refineMesh -overwrite
  # topoSet    
setsToZones -noFlipMap

createPatch -overwrite

//setWaveParameters 
setFields

//setWaveField 
waveGaugesNProbes


decomposePar

echo /home/wisney/OpenFOAM/wisney-v1812/run/porosity/porousforce/U/U20/processor*/constant/ | xargs -n 1 cp /home/wisney/OpenFOAM/wisney-v1812/run/porosity/porousforce/U/U20/constant/porosityZones


mpirun -np 6 porousWaveFoam -parallel |tee log.run
decomposePar
mpirun -np 18 waveFoam -parallel |tee log.run

reconstructPar

ffmpeg -y -r 10 -i a.%04d.jpg -vcodec libx264 output.mp4
