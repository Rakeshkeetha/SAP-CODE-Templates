REPORT ztable_navigation_launcher.

*---------------------------------------------------------------------*
* Text Symbols
*---------------------------------------------------------------------*
TEXT-001 = 'ZUIWC Contribution'.
TEXT-002 = 'ZUIWC Invoice'.
TEXT-003 = 'ZUIWC Config'.
TEXT-004 = 'ZUIWC Agency'.

*---------------------------------------------------------------------*
* Selection Screen
*---------------------------------------------------------------------*
SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE text-t01.

SELECTION-SCREEN PUSHBUTTON 5(30) btn_contr USER-COMMAND c1.
SELECTION-SCREEN PUSHBUTTON 5(30) btn_invoice USER-COMMAND c2.
SELECTION-SCREEN PUSHBUTTON 5(30) btn_config USER-COMMAND c3.
SELECTION-SCREEN PUSHBUTTON 5(30) btn_agency USER-COMMAND c4.

SELECTION-SCREEN END OF BLOCK b1.

*---------------------------------------------------------------------*
* Initialization
*---------------------------------------------------------------------*
INITIALIZATION.
  btn_contr   = TEXT-001.
  btn_invoice = TEXT-002.
  btn_config  = TEXT-003.
  btn_agency  = TEXT-004.

*---------------------------------------------------------------------*
* Selection Screen Event
*---------------------------------------------------------------------*
AT SELECTION-SCREEN.
  CASE sy-ucomm.
    WHEN 'C1'.
      PERFORM display_table USING 'ZUIWC_CONTR'.
    WHEN 'C2'.
      PERFORM display_table USING 'ZUIWC_INVOICE'.
    WHEN 'C3'.
      PERFORM display_table USING 'ZUIWC_CONFIG'.
    WHEN 'C4'.
      PERFORM display_table USING 'ZUIWC_AGENCY'.
  ENDCASE.

*---------------------------------------------------------------------*
* Forms
*---------------------------------------------------------------------*
FORM display_table USING iv_tabname TYPE tabname.

  DATA: lt_param TYPE TABLE OF rsparams,
        ls_param TYPE rsparams.

  CLEAR lt_param.

  ls_param-selname = 'DTAB'.
  ls_param-kind    = 'P'.
  ls_param-low     = iv_tabname.
  APPEND ls_param TO lt_param.

  CALL TRANSACTION 'SE16N'
    WITH SELECTION-TABLE lt_param
    AND SKIP FIRST SCREEN.

ENDFORM.
