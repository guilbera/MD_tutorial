# MD_tutorial

You will need to download and install VMD (http://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD) and gromacs 4.6.5 (http://manual.gromacs.org/documentation/).

VMD enables to visualise molecules and trajectories, gromacs is the MD package.

To start,
go to the folder MD_tutorial. There are two .pdb files with the initial microstate of a 3hexylthiophene oligomer (P3HT.pdb) and a Phenyl-C61-butyric acid methyl ester molecule (PCBM.pdb). You can start VMD and load the file by going to File/New Molecule ... 

Now let do some MD. Gromacs is using command lines. 

1. echo 5 | pdb2gmx -f P3HT.pdb -ff gmx

The force field is stored in the gmx.ff folder and the inital P3HT microstate in P3HT.pdb. pdb2gmx links these two files and create a topology file (.top) where all the informations related to this model is stored. He also transform the .pdb file to a .gro file containing the same information (conf.gro gromacs format).

2. grompp -f energy_minimisation.mdp

The .mdp contains all the parameters for the simulation. Here we are first going to minimise the energy using a steepest descent algorithm. This ensures that if a bon for instance is too long, it is going to be corrected before evolving our system. This step only prepares the simulation linking topology and parameters in a .tpr file.

3. mdrun -s topol.tpr -v 

This is going to run the energy minimisation. -v option allows to see what's happening at every step. It creates a new confout.gro file that you can visualise with vmd. This should be slightly different from your initial P3HT.pdb file.

4. grompp -f NVE.mdp -c confout.gro

Same as step 2 except that we are taking as initial structure the energy minimised one (confout.gro) and we have new parameters (integrator to do a md simulation).

5. mdrun -s topol.tpr -v

Same as 3 except this time it runs a md simulation. It creates a traj.trr file, which is the trajectory in NVE of our P3HT molecule. You can visualise the trjaectory using vmd (type the command vmd traj.trr confout.gro). You can play the trajectory by clicking on the small arrow at the bottom right corner.

6. You can repeat the same with NVT.mdp to add temperature effect. You can rerun the same thing with PCBM.pdb.

7. We can do a simulation with a P3HT and a PCBM molecule. For this, we can use:

genbox -cp P3HT.pdb -ci PCBM.pdb -box 10 -nmol 1

echo 5 | pdb2gmx -f out.gro -ff gmx

and then follow 2-5
You may want to make the molecule whole again (molecules being split at the border) by using:

echo 0 | trjconv -f traj.trr -pbc whole

It creates a new trajecotry in a compressed format trajout.xtc
Again you can visualise it using vmd trajout.xtc conf.gro

8. Finally we can look at energies using:

g_energy -f ener.edr

You should be asked what energy you want to extract. It will gives you the average over time and also creates a energy.xvg file. By plotting the data you can see how the selected energies vary with time. 

Lots of other analysis can be done using gromacs subroutine. For more information, you can have a look at gromacs manual (ftp://ftp.gromacs.org/pub/manual/manual-4.6.5.pdf). They are listed and explained in the Appendix D.





