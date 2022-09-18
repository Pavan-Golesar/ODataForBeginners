# ODataForBeginners
OData For Beginners

READ
  METHOD travelagencyset_get_entity.

    DATA: ls_key_tab   TYPE /iwbep/s_mgw_name_value_pair,
          ls_entityset TYPE stravelag.

* key is TravelAgencySet(agency#)
    READ TABLE it_key_tab INTO ls_key_tab INDEX 1.

    SELECT SINGLE * FROM stravelag INTO ls_entityset WHERE agencynum = ls_key_tab-value.

    IF sy-subrc NE 0.
      DATA: lo_message_container    TYPE REF TO /iwbep/if_message_container.
      lo_message_container = me->mo_context->get_message_container( ).

      lo_message_container = me->mo_context->get_message_container( ).

      CALL METHOD lo_message_container->add_message_text_only
        EXPORTING
          iv_msg_type               = 'E'
          iv_msg_text               = 'No Data!!!'
          iv_add_to_response_header = abap_true.

      RAISE EXCEPTION TYPE /iwbep/cx_mgw_busi_exception
        EXPORTING
          message_container = lo_message_container.
    ENDIF.

    er_entity = ls_entityset.

  ENDMETHOD.

Delete
METHOD travelagencyset_delete_entity.

  DATA: ls_entityset    TYPE stravelag,
        ls_key_tab      TYPE /iwbep/s_mgw_name_value_pair,
        lv_error_entity TYPE string.

* key is TravelAgencySet(agency#)
  READ TABLE it_key_tab INTO ls_key_tab INDEX 1.

  DELETE FROM stravelag WHERE agencynum = ls_key_tab-value.

  IF ( sy-subrc = 0 ).
* delete completed
      else.
* entity not found
        concatenate iv_entity_name
                    '('''
                    ls_key_tab-value
                    ''')'
          into lv_error_entity.
    RAISE EXCEPTION TYPE /iwbep/cx_mgw_busi_exception
      EXPORTING
        textid      = /iwbep/cx_mgw_busi_exception=>resource_not_found
        entity_type = lv_error_entity.
  ENDIF.

ENDMETHOD.


Query
METHOD travelagencyset_get_entityset.

    DATA: lv_osql_where_clause TYPE string.
    lv_osql_where_clause = io_tech_request_context->get_osql_where_clause( ).

    SELECT * FROM stravelag INTO CORRESPONDING FIELDS OF TABLE @et_entityset
      WHERE (lv_osql_where_clause).

ENDMETHOD.


Create
method TRAVELAGENCYSET_CREATE_ENTITY.

DATA: ls_entityset    TYPE stravelag,
        ls_key_tab      TYPE /iwbep/s_mgw_name_value_pair,
        lv_error_entity TYPE string.

  io_data_provider->read_entry_data( IMPORTING es_data = ls_entityset ).

  ls_key_tab-value = ls_entityset-agencynum.

  INSERT into stravelag values ls_entityset.

  IF ( sy-subrc = 0 ).
* entity inserted
    er_entity = ls_entityset.
  ELSE.
* entity alredy exists
    CONCATENATE iv_entity_name
                '('''
                ls_key_tab-value
                ''')'
      INTO lv_error_entity.
    RAISE EXCEPTION TYPE /iwbep/cx_mgw_busi_exception
      EXPORTING
        textid      = /iwbep/cx_mgw_busi_exception=>resource_duplicate
        entity_type = lv_error_entity.
  ENDIF.

endmethod.


Sample OData Post Payload JSON fomrat:
{
  "d" : {
    "ContractAccountID" : "70000014236",
    "Class" : "AUTH",
    "Action" : "1"
}
}


Update
  METHOD travelagencyset_update_entity.

    DATA: ls_entityset    TYPE stravelag,
          ls_key_tab      TYPE /iwbep/s_mgw_name_value_pair,
          lv_error_entity TYPE string.

    io_data_provider->read_entry_data( IMPORTING es_data = ls_entityset ).

* key is TravelAgencySet(agency#)
    READ TABLE it_key_tab INTO ls_key_tab INDEX 1.

* make sure the value matches with the one in OData payload
    IF ls_key_tab-value EQ ls_entityset-agencynum.

* new data
      UPDATE stravelag FROM ls_entityset.

      IF ( sy-subrc = 0 ).
* entity found and updated
        er_entity = ls_entityset.
      ELSE.
* entity not found
        DATA: lo_message_container    TYPE REF TO /iwbep/if_message_container.
        lo_message_container = me->mo_context->get_message_container( ).

        lo_message_container = me->mo_context->get_message_container( ).

        CALL METHOD lo_message_container->add_message_text_only
          EXPORTING
            iv_msg_type               = 'E'
            iv_msg_text               = 'No update performed!!!'
            iv_add_to_response_header = abap_true.

        RAISE EXCEPTION TYPE /iwbep/cx_mgw_busi_exception
          EXPORTING
            message_container = lo_message_container.
      ENDIF.

    ENDIF.

  ENDMETHOD.![image](https://user-images.githubusercontent.com/25543125/190902231-306dc067-8b15-411a-9747-25e10d5f9674.png)
