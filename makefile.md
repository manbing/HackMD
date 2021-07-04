# GNU Makefile
###### tags: `GNU`

# The call Function
```
ifndef irene
define irene
        @echo $(0) $(1) $(2)
endef
endif


all:
        $(call irene,is,girl)
```
