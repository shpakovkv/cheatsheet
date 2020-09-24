RD53a Travelling Modules stage - Tuning routine (commands)
----------------------------------------------------------

##### Pre-tuning scans:

bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_digitalscan.json -p -m 1
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_analogscan.json -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_thresholdscan.json -p

##### Differential FE tuning:
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/diff_tune_globalthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/diff_tune_pixelthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/diff_tune_globalpreamp.json -t 10000 8 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/diff_tune_pixelthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/diff_tune_finepixelthreshold.json -t 1000 -p

##### Linear FE tuning:
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_tune_globalthreshold.json -t 2000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_tune_pixelthreshold.json -t 2000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_retune_globalthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_retune_pixelthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_tune_globalpreamp.json -t 10000 8 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_retune_pixelthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/lin_tune_finepixelthreshold.json -t 1000 -p

##### Synchronous FE tuning:
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/syn_tune_globalthreshold.json -t 1000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/syn_tune_globalpreamp.json -t 10000 8 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/syn_tune_globalthreshold.json -t 1000 -p

##### Post-tuning scans
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_thresholdscan.json -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_totscan.json -t 10000 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_noisescan.json -p

##### Optional (reset mask before noise scan):
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_digitalscan.json -m 1 -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_analogscan.json -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_noisescan.json -p

##### Search for folders with Threshold and Noise data files:
find data/TravellingChip_0x1F72/ -name "*ThresholdMap*.dat"
find data/TravellingChip_0x1F72/ -name "*NoiseMap*.dat"

##### PlotWithRoot Threshold 
./plotting/plotWithRoot_Threshold data/TravellingChip_0x1F72/000052_std_thresholdscan/ .dat 1
./plotting/plotWithRoot_Threshold data/TravellingChip_0x1F72/000057_diff_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_Threshold data/TravellingChip_0x1F72/000064_lin_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_Threshold data/TravellingChip_0x1F72/000068_std_thresholdscan/ .dat 1

##### PlotWithRoot NoiseMap
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000052_std_thresholdscan/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000057_diff_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000057_diff_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000057_diff_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000064_lin_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000064_lin_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000064_lin_tune_finepixelthreshold/ .dat 1
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000068_std_thresholdscan/ .dat 1


===============================================================================================================================================================================================
travell_rd53a_setup_temp.json

bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup_temp.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_digitalscan.json -p -m 1
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup_temp.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_analogscan.json -p
bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup_temp.json -o ./data/TravellingChip_0x1F72 -s configs/scans/rd53a/std_thresholdscan.json -p

./plotting/plotWithRoot_Threshold data/TravellingChip_0x1F72/000077_std_thresholdscan/ .dat 1 
./plotting/plotWithRoot_Threshold data/TravellingChip_0x1F72/000080_std_thresholdscan/ .dat 1 
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000077_std_thresholdscan/ .dat 1 
./plotting/plotWithRoot_NoiseMap data/TravellingChip_0x1F72/000080_std_thresholdscan/ .dat 1

