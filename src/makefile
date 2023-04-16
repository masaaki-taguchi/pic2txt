# この makefile は、msys や cygwin などの Unix 互換環境上で利用することを想定している。
# ビルドには xdev68k が必要。
# https://github.com/yosshin4004/xdev68k

# 必要な環境変数が定義されていることを確認する。
ifndef XDEV68K_DIR
	$(error ERROR : XDEV68K_DIR is not defined.)
endif

# デフォルトサフィックスを削除
.SUFFIXES:

# 各種コマンド短縮名
RUN68 = ${XDEV68K_DIR}/run68/run68
HAS = $(RUN68) ${XDEV68K_DIR}/x68k_bin/HAS060.X
HLK = $(RUN68) ${XDEV68K_DIR}/x68k_bin/hlk301.x
CV = $(RUN68) ${XDEV68K_DIR}/x68k_bin/CV.X

# プログラム名(拡張子除く)
PROG = ../PIC2TXT

# ヘッダ検索パス
INCLUDE_FLAGS = -I${XDEV68K_DIR}/include/xc -I${XDEV68K_DIR}/include/misc

# *.HAS ソースファイル
ASM_SRCS = PIC2TXT.HAS

# リンク対象のライブラリファイル
LIBS =\
	${XDEV68K_DIR}/lib/misc/APICGLIB.a \

# 中間ファイル生成用ディレクトリ
INTERMEDIATE_DIR = _build

# オブジェクトファイル
OBJS =	$(addprefix $(INTERMEDIATE_DIR)/,$(patsubst %.HAS,%.o,$(ASM_SRCS)))

# HLK に入力するリンクリスト
HLK_LINK_LIST = $(INTERMEDIATE_DIR)/_lk_list.tmp

# デフォルトのターゲット
all : $(PROG).R

# 中間生成物の削除
clean : 
	rm -f $(PROG).R
	rm -rf $(INTERMEDIATE_DIR)

# 実行ファイルの生成
#	HLK に長いパス文字を与えることは難しい。
#	回避策としてリンク対象ファイルを $(INTERMEDIATE_DIR) 以下にコピーし、
#	短い相対パスを用いてリンクを実行させる。
$(PROG).R : $(OBJS)
	mkdir -p $(INTERMEDIATE_DIR)
	rm -f $(HLK_LINK_LIST)
	@for FILENAME in $(OBJS); do\
		echo $$FILENAME >> $(HLK_LINK_LIST); \
        done
	@for FILENAME in $(LIBS); do\
		cp $$FILENAME $(INTERMEDIATE_DIR)/`basename $$FILENAME`; \
		echo $(INTERMEDIATE_DIR)/`basename $$FILENAME` >> $(HLK_LINK_LIST); \
        done
	$(HLK) -i $(HLK_LINK_LIST) -o $(PROG).X
	$(CV) $(PROG).X
	mv $(PROG).r $(PROG).R
	rm -f $(PROG).X

# *.HAS ソースのアセンブル
$(INTERMEDIATE_DIR)/%.o : %.HAS makefile
	mkdir -p $(INTERMEDIATE_DIR)
	$(HAS) -e -u -w0 $(INCLUDE_FLAGS) $*.HAS -o $(INTERMEDIATE_DIR)/$*.o

$(INTERMEDIATE_DIR)/PIC2TXT.o:	PIC2TXT.HAS