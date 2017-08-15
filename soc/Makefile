PRJ=DE0_HSP_Example

QPF := $(PRJ).qpf
QSF := $(PRJ).qsys
QSD := $(PRJ)

QIP_FILE  		:= $(QSD)/synthesis/$(PRJ).qip
SOF_FILE  		:= $(PRJ).sof
SOPC_FILE 		:= $(PRJ).sopcinfo
DTS_FILE  		:= $(PRJ).dts
BINFO_FILE 		:= board_info.xml
DTB_FILE 		:= $(PRJ).dtb
RBF_FILE 		:= $(PRJ).rbf
H_DIR 			:= qsys_headers

SOPC2DTS_ARGS += --bridge-removal all --clocks

CMD_MKDIR := mkdir -p



.PHONY: quartus_edit
quartus_edit:
	quartus $(QPF) &

.PHONY: qsys_edit
qsys_edit:
	qsys-edit $(QSF) &

.PHONY: qsys_generate
qsys_generate: $(QIP_FILE)

.PHONY: quartus_generate
quartus_generate:  $(SOF_FILE)

.PHONY: device_tree
device_tree: $(DTS_FILE)

.PHONY: blob
blob: $(DTB_FILE)

.PHONY: headers
headers: $(H_DIR)/$(PRJ).h

.PHONY: rbf
rbf: $(RBF_FILE)


$(RBF_FILE): $(SOF_FILE)
	quartus_cpf -c $(SOF_FILE) $(RBF_FILE)

$(DTB_FILE): $(DTS_FILE)
	dtc -I dts -O dtb -o $(DTB_FILE) $(DTS_FILE)

device_tree_edit:
	sopc2dts -i $(SOPC_FILE) --board $(BINFO_FILE) --gui

$(DTS_FILE): $(SOPC_FILE) $(BINFO_FILE)
	sopc2dts -i $(SOPC_FILE) -o $(DTS_FILE) $(OPC2DTS_ARGS) --board $(BINFO_FILE)

$(H_DIR)/$(PRJ).h: $(SOPC_FILE)
	$(CMD_MKDIR) $(H_DIR)
	sopc-create-header-files $(SOPC_FILE) --output-dir $(H_DIR)

$(QIP_FILE): $(QSF)
	qsys-generate $(QSF) --synthesis=VERILOG --output-directory=$(QSD)

$(SOF_FILE): $(QIP_FILE)
	quartus_sh --flow compile  $(QPF)


clean:
	rm -rf 	db \
			hps_isw_handoff \
			incremental_db \
	        c5_pin_model_dump.txt \
	        hps_sdram_p0_all_pins.txt \
	        hps_sdram_p0_summary.csv \
	        DE0_HSP_Example_assignment_defaults.qdf \
			.qsys_edit \
	  		$(QSD) \
			$(SOF_FILE) \
			$(DTS_FILE) \
			$(DTB_FILE) \
			$(H_DIR)\
			$(RBF_FILE)\
			*.rpt \
			*.done \
			*.smsg \
			*.summary \
			*.htm \
			*.sopcinfo \
			*.sdl \
			*.pin \
			*.qws \
			*.sld \
			*.jdi 


