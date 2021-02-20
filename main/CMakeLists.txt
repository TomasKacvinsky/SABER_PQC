idf_component_register(
	SRCS 		cbd.c fips202.c kem.c pack_unpack.c poly.c poly_mul.c SABER_indcpa.c saberwintestkex.c verify.c
        INCLUDE_DIRS 	.
			~/.espressif/tools/xtensa-esp32s2-elf/esp-2020r2-8.2.0/xtensa-esp32s2-elf/xtensa-esp32s2-elf/include
			../../../../components/xtensa/include
			../../../../components/freertos/include
			../../../../components/heap/include
			../components/wolfssl/wolfssl
	PRIV_REQUIRES	wolfssl
	)

if (B_FLAGS)
	string(REPLACE " " ";" B2_FLAGS ${B_FLAGS})
	target_compile_options(${COMPONENT_LIB} PRIVATE ${B2_FLAGS})
endif()

list(FIND B2_FLAGS "-DMY_ESP_MALLOC" POSITION)
if (POSITION EQUAL -1)
	target_compile_options(${COMPONENT_LIB} PUBLIC -Daligned_alloc\(x,y\)=malloc\(y\))
else()
	target_compile_options(${COMPONENT_LIB} PUBLIC -Daligned_alloc\(x,y\)=my_ESP_malloc\(y\))
	target_compile_options(${COMPONENT_LIB} PUBLIC -Dmalloc\(y\)=my_ESP_malloc\(y\))
	target_compile_options(${COMPONENT_LIB} PUBLIC -Dfree\(y\)=my_ESP_free\(y\))
endif()
