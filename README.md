# ODataForBeginners
OData For Beginners

![image](https://user-images.githubusercontent.com/25543125/190910988-24e03abc-c684-4f5b-9c75-f250b7208278.png)
_________________________________________________________________________________________________________________________________________________________________
JSON FORMAT

{

  "d" : {
  
    "Name" : "Pavan Golesar",
    
    "Email" : "sapparamount@gmail.com",
    
    "LogonDate" : "\\/Date(1652486400000)\\/",
    
    "Tech" : "SAP"
    
}

}

<img width="704" alt="image" src="https://user-images.githubusercontent.com/25543125/204491566-b400d0a5-8957-4bd0-ae55-1eb9a9d3b5a6.png">

_________________________________________________________________________________________________________________________________________________________________

---------------------> DEEP INSERT<----------------

![image](https://user-images.githubusercontent.com/25543125/191527130-67d8c3a0-798c-4e47-9f16-aa9cfaa9fb8b.png)


* MPC_EXT New 'Type':

TYPES: BEGIN OF ts_deep_insert.

    INCLUDE TYPE zcl_zdemo13_mpc_ext=>ts_header.
    TYPES: NavItem TYPE STANDARD TABLE OF ts_item WITH DEFAULT KEY,

END OF ts_deep_insert.
    
________________________________________________________________________________________________________________________________________________

method DEFINE.

    DATA lo_entity_type TYPE REF TO /iwbep/if_mgw_odata_entity_typ.

    super->define( ).
    
    lo_entity_type = model->get_entity_type( iv_entity_name = 'Header' ).
    
    lo_entity_type->bind_structure( iv_structure_name = 'ZCL_ZDEMO13_MPC_EXT=>TS_DEEP_INSERT').

endmethod.
________________________________________________________________________________________________________________________________________________  

$BATCH: multipart/mixed; boundary=batch

![image](https://user-images.githubusercontent.com/25543125/190911060-d4189297-7e9c-41de-be77-c308174b0f5b.png)

![image](https://user-images.githubusercontent.com/25543125/190911872-2d3b1c24-fc34-40b2-b936-07619701f675.png)

Please write back to us at sapparamount@gmail.com.

LinkedIn
linkedin.com/pulse/sap-btp-abap-environment-series-pavan-golesar

Medium
medium.com/@pavangolesar/exploring-sap-btp-abap-environment-with-pavan-golesar-72d5e63ae580

SlideShare
slideshare.net/PavanGolesar

Twitter
twitter.com/SAP_Cr7

Explore16k Music Production
soundcloud.com/ethic-coder-pavan-golesar
