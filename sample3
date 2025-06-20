*&---------------------------------------------------------------------*
*& Include          YSAMPLE1RD01
*&---------------------------------------------------------------------*
TYPE-POOLS:SLIS.

TABLES:VBAK,VBAP,VBPA,VBEP.

TYPES:
  TYP_RETURN TYPE RANGE OF VBFA-VBTYP_N,
  TYP_BOCATE TYPE RANGE OF VBAK-AUART,
  TYP_TT_ALV TYPE STANDARD TABLE OF YZCHEN2_1R_ALV.

DATA:
  GIT_IMFIXEDVALUE TYPE   ZAMCMTD_EV0001,         "固定値取得用内部テーブル
  GR_IT_RETURN        TYPE TYP_RETURN,                     "レンジテーブル: 返品伝票
  GR_W_RETURN         LIKE LINE OF GR_IT_RETURN,     "構造: 返品伝票
  GR_IT_BOCATE        TYPE TYP_BOCATE,                   "レンジテーブル: 請求伝票カテゴリ
  GR_W_BOCATE         LIKE LINE OF GR_IT_BOCATE,     "構造: 請求伝票カテゴリ
  GR_IT_DOSTATUS    TYPE RANGE OF C,                     "レンジテーブル: 出荷ステータス
  GR_W_DOSTATUS     LIKE LINE OF GR_IT_DOSTATUS, "構造: 出荷ステータス
  GR_IT_BOSTATUS    TYPE RANGE OF C,                     "レンジテーブル: 請求ステータス
  GR_W_BOSTATUS     LIKE LINE OF GR_IT_BOSTATUS, "構造: 請求ステータス
  GIT_FIELDCAT          TYPE SLIS_T_FIELDCAT_ALV,         "ALV表示項目
  GIT_TBL_ALVDATA    TYPE TYP_TT_ALV.                       "ALV出力リスト


*受注伝票情報構造
TYPES: BEGIN OF TYP_W_SALES,
         AUDAT  LIKE VBAK-AUDAT,  " 伝票日付
         VKGRP  LIKE VBAK-VKGRP,  " 営業グループ
         AUART  LIKE VBAK-AUART,  " 販売伝票タイプ
         VBELN  LIKE VBAK-VBELN,  " 販売伝票
         POSNR  LIKE VBAP-POSNR,  " 販売伝票明細
         EDATU  LIKE VBEP-EDATU,  " 納入日程日付
         LFSTA  LIKE VBAP-LFSTA,  " 出荷ステータス (明細)
         LIFSP  LIKE VBEP-LIFSP,  " 出荷ブロック中の納入日程行
         FAKSK  LIKE VBAK-FAKSK,  " 出荷ブロック (伝票ヘッダ)
         FAKSP  LIKE VBAP-FAKSP,  " 明細の請求ブロック
         ABGRU  LIKE VBAP-ABGRU,  " 販売伝票の拒否理由
         ERNAM  LIKE VBAK-ERNAM,  " オブジェクトの登録担当者の名前
         ERDAT  LIKE VBAK-ERDAT,  " レコードが登録された日付
         ERZET  LIKE VBAK-ERZET,  " 登録時刻
         PSTYV  LIKE VBAP-PSTYV,  " 明細カテゴリ (販売伝票)
         LGORT  LIKE VBAP-LGORT,  " 保管場所
         MATNR  LIKE VBAP-MATNR,  " 品目コード
         KWMENG LIKE VBAP-KWMENG,  "  累積受注数量 (販売単位)
         FKSAA  LIKE VBAP-FKSAA,  " 受注関連の請求ステータス (明細)
         NETWR  LIKE VBAP-NETWR,  " 伝票通貨での受注明細正味額
         NETPR  LIKE VBAP-NETPR,  " 正味価格
         WAERK  LIKE VBAP-WAERK,  " 販売管理伝票通貨
         MWSBP  LIKE VBAP-MWSBP,  " 伝票通貨での税額
       END OF TYP_W_SALES,
       TYP_TT_SALES TYPE STANDARD TABLE OF TYP_W_SALES.
DATA: GIT_TBL_SALES TYPE TYP_TT_SALES.

*出荷伝票情報構造
TYPES: BEGIN OF TYP_W_DELIVERY ,
         VBELN LIKE LIPS-VBELN,  "  出荷伝票
         POSNR LIKE LIPS-POSNR,  "  出荷明細
         VGBEL LIKE LIPS-VGBEL,  "  参照伝票番号
         VGPOS LIKE LIPS-VGPOS,  "  参照明細番号
         ERDAT LIKE LIPS-ERDAT,  "  レコードが登録された日付
       END OF TYP_W_DELIVERY,
       TYP_TT_DELIVERY TYPE STANDARD TABLE OF TYP_W_DELIVERY
     WITH NON-UNIQUE SORTED KEY NS01 COMPONENTS
     VGBEL
     VGPOS.
DATA:  GIT_TBL_DELIVERY TYPE  TYP_TT_DELIVERY.

*請求伝票情報構造
TYPES: BEGIN OF TYP_W_BILLING,
         VBELN LIKE                      VBRK-VBELN,  " 請求伝票
         POSNR LIKE                      VBRP-POSNR,  " 請求明細
         FKDAT LIKE                      VBRK-FKDAT,  " 請求日付
         AUBEL LIKE                      VBRP-AUBEL,  " 販売伝票
         AUPOS LIKE                      VBRP-AUPOS,  " 販売伝票明細
       END OF   TYP_W_BILLING,
       TYP_TT_BILLING TYPE STANDARD TABLE OF TYP_W_BILLING
     WITH NON-UNIQUE SORTED KEY NS01 COMPONENTS
     AUBEL
     AUPOS.
DATA : GIT_TBL_BILLING TYPE TYP_TT_BILLING.

*取引先機能情報構造
TYPES:BEGIN OF TYP_W_PARTNER,
        VBELN      LIKE                     VBPA-VBELN,  "  販売管理伝票番号
        POSNR      LIKE                    VBPA-POSNR,  "  明細番号 (販売管理伝票)
        KUNNR      LIKE                     VBPA-KUNNR,  "  得意先コード
        PARVW      LIKE                    VBPA-PARVW,  "  取引先機能
        NAME1      LIKE                    ADRC-NAME1,  "  名称 1
        STREET     LIKE                    ADRC-STREET,  " 地名
        CITY1      LIKE                      ADRC-CITY1,  "  市区町村
        CITY2      LIKE                     ADRC-CITY2,  "  所在地
        STR_SUPPL1 LIKE                ADRC-STR_SUPPL1,  " 名称 1
        STR_SUPPL2 LIKE                 ADRC-STR_SUPPL2,  " 明細番号 (販売管理伝票)
        POST_CODE1 LIKE                ADRC-POST_CODE1,  " 市区町村の郵便番号
      END OF  TYP_W_PARTNER,
      TYP_TT_PARTNER TYPE STANDARD TABLE OF TYP_W_PARTNER
     WITH NON-UNIQUE SORTED KEY NS01 COMPONENTS
     VBELN
     PARVW.
DATA:  GIT_TBL_PARTNER TYPE TYP_TT_PARTNER.

*品目マスタ情報構造
TYPES:BEGIN OF TYP_W_MATERIAL,
        MATNR LIKE                    MARA-MATNR, "  品目コード
        MTART LIKE                    MARA-MTART, "  品目タイプ
        MAKTX LIKE                    MAKT-MAKTX, "  品目テキスト
      END OF   TYP_W_MATERIAL,
      TYP_TT_MATERIAL TYPE STANDARD TABLE OF TYP_W_MATERIAL
     WITH NON-UNIQUE SORTED KEY NS01 COMPONENTS
     MATNR.
DATA: GIT_TBL_MATERIAL TYPE TYP_TT_MATERIAL.

CONSTANTS:
  CNS_RICEFID TYPE ZAME_RICEFID           VALUE 'SD0001',   "RICEFID
  CNS_WE         LIKE  VBPA-PARVW            VALUE 'WE',          "出荷先
  CNS_AG         LIKE  VBPA-PARVW            VALUE 'AG',          "受注先
  CNS_RE         LIKE  VBPA-PARVW            VALUE 'RE',          "請求先
  CNS_RG         LIKE  VBPA-PARVW            VALUE 'RG',          "支払人
  CNS_0            LIKE  VBPA-POSNR            VALUE '000000',   "明細番号 (販売管理伝票))
  CNS_ZFXX(4) VALUE 'ZFXX'.                                                 "請求タイプ前受金
