# porouswavefoam
#It can generate stable wind fields and couple porous medium models, while allowing for the free definition of horizontal wave-overrun monitoring surfaces at different spatial locations behind the embankment, as well as pressure measurement points on the ground behind the embankment.

#The tutorials contain files for arranging the pressure points behind the dike and the over-wave detection surface for different algorithms, and the geometry of the seawall can be modified by modifying the blockMeshDict file.

#Here are some of the commands that may be used

blockMesh

topoSet  

subsetMesh obstacle  -overwrite -patch bottom

refineMesh -overwrite

setsToZones -noFlipMap

createPatch -overwrite

setWaveParameters 

setWaveField 

waveGaugesNProbes

#parallel computing
decomposePar

echo /home/wisney/OpenFOAM/wisney-v1812/run/porosity/porousforce/U/U20/processor*/constant/ | xargs -n 1 cp /home/wisney/OpenFOAM/wisney-v1812/run/porosity/porousforce/U/U20/constant/porosityZones

mpirun -np 18  porousWaveFoam -parallel |tee log.run

#decomposePar

#mpirun -np 18 waveFoam -parallel |tee log.run

reconstructPar

#Exporting images for animation

ffmpeg -y -r 10 -i a.%04d.jpg -vcodec libx264 output.mp4

