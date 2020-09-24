RD53a Travelling Modules stage - Tuning routine (checklist)
----------------------------------------------------------

##### Pre-tuning scans:
- [ ] std_digitalscan.json 	-p -m 1
- [ ] std_analogscan.json 	-p
- [ ] std_thresholdscan.json 	-p

##### Differential FE tuning:
- [ ] diff_tune_globalthreshold.json 	-t 1000 -p
- [ ] diff_tune_pixelthreshold.json 	-t 1000 -p
- [ ] diff_tune_globalpreamp.json 	-t 10000 8 -p
- [ ] diff_tune_pixelthreshold.json 	-t 1000 -p
- [ ] diff_tune_finepixelthreshold.json 	-t 1000 -p

##### Linear FE tuning:
- [ ] lin_tune_globalthreshold.json 	-t 2000 -p
- [ ] lin_tune_pixelthreshold.json 	-t 2000 -p
- [ ] lin_retune_globalthreshold.json -t 1000 -p
- [ ] lin_retune_pixelthreshold.json 	-t 1000 -p
- [ ] lin_tune_globalpreamp.json 		-t 10000 8 -p
- [ ] lin_retune_pixelthreshold.json 	-t 1000 -p
- [ ] lin_tune_finepixelthreshold.json -t 1000 -p

##### Synchronous FE tuning:
- [ ] syn_tune_globalthreshold.json 	-t 1000 -p
- [ ] syn_tune_globalpreamp.json 		-t 10000 8 -p
- [ ] syn_tune_globalthreshold.json 	-t 1000 -p

##### Post-tuning scans
- [ ] std_thresholdscan.json 	-p
- [ ] std_totscan.json 		-t 10000 -p
- [ ] std_noisescan.json 		-p

##### Optional
- [ ] Look into the pixel configuration section: if all pixels are disabled, simply run another digital scan with -m 1 to reset the enable mask followed by a normal analog scan:
- [ ] std_digitalscan.json -m 1 -p
- [ ] std_analogscan.json  -p
- [ ] std_noisescan.json 	 -p
