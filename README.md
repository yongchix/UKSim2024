# UKSim2024
This is the generic version for simulating both neutrons and attenuation of (n,n'g).  

	1. make a subfolder, e.g. "build" inside the top level directory
	2. enter this subfolder, do "cmake .."
	3. make sure all required geant4 *.mac macros, bash macros and associated programs are available in this folder
		a. geant4 macros: 
			i. attenuation of neutrons: 
				1) n_dist.mac: defines how and how much neutrons are generated for simulating its scattering with the sample
				2) n_energy.mac: defines the energy of mono-energetic neutrons
				3) ini_neutrons.mac: needed by running simulations for neutron scatterings. 
			ii. attenuation of gammas: 
				1) g_dir.mac: set the arc(tain) value of the angle at which the gamma rays are pointing to the detector
				2) g_emission.mac: set the position as well as number of emissions of point-like gamma sources inside the volume occupied by the scattering sample. The exact position is determined by the size of the voxels making up the scattering sample. The number of emissions inside each voxel is determined by both the voxel size and the number of interactions of neutrons inside the sample. (The size of the voxel is typically 1 mm^3)
				3) g_energy.mac: set the gamma ray energy
				4) g_gps_sample_ON/OFF.mac: set all other attributes of gamma sources. The difference is on whether or not the /gps/pos/confine UKALSample is commented out or not. 
				5) ini_gamma_sample_ON/OFF.mac: needed by running simulations for gamma attenuations, depending on whether or not the sample is in its place. 
			iii. generic: 
				1) rotate_det.mac: defines the angle of the first detector with respect to the direction of the beam line
				2) vis.mac: for visulation of the simulations, keep it as it is. 
		b.  bash macros: 
				1) ADD_container.bash
				2) REMOVE_container.bash
				3) make_neutron_ini_macs.bash
				4) run_neutrons.bash: the user may set the array of neutron energies in this macro as well as the detection of the angle so that these two parameters may be written into associated geant4 macros. The user just needs to run: ./run_neutrons.bash <sample name> to call the simulation program "sim_neutrons"
				5) run_gammas.bash: the user needs to set the array of neutron energies in this macro so that the macro will loop over all energies and all directions at which the detector points to the sample and run corresponding simulations. 
		c. programs:  "VoxelMaker"
			i. This is a compiled program for which the source code and Makefile is located in a folder called "voxel_maker". 
			ii. By default the size of a voxel is 1 mm^3. It counts the number of single/multiple/unconditional scatterings inside a voxel. 
			iii. The program has to be in the same level of folder as the above bash macros. 
	4. after the building through cmake is done, do "make -jN", where N is the number of threads used for this build parallelly 
		a. then a program named as "sim_" should appear in the current directory
		b. rename this "sim_" to either "sim_gammas" or "sim_neutrons", which are to be called by "run_gammas.bash" and "run_neutrons.bash" respectively. These two programs are to simulate gamma attenuations through the sample and the neutron multiple scatterings inside the sample. 
	5. run the simulation: 
		a. neutrons:  ./run_neutrons.bash <sample name> 
		b. gammas: ./run_gammas.bash <sample name> 
		
![image](https://github.com/yongchix/UKSim2024/assets/8482708/a8690c40-78c7-46b9-95ee-c66c0134e204)
