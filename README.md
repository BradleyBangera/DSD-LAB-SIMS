# DSD-LAB-SIMS
DSD LAB SIMULATIONS
.Design and test a circuit 2-Bit synchronous up-counter which counts the
sequence 00->01->10->00->01->10 using D Flip Flop

PR=1 CLR=1 Q1=0 Q2=0
CLK=1 PULSE Q1=0 Q2=1
CLK=2 PULSE Q1=1 Q2=0


2.Design and test a circuit that checks for the sequence in 010 continuously in
a data sequence using a JKFF,implement the design using a mealy machine

use this sequence to run

1) Set PR=1 CLR=1

2)X=0 CLK= 1 pulse Q1=0 Q2=1 Y=0

3)X=1 CLK =2ND PULSE Q1=1 Q2=0 Y=0

4) CLK=NO CHANGE X=0 Y=1
Mc
  org 0000h
ljmp main
#define lcdinit 0e267h
#define lcdcmd 0e295h
#define lcddat 0e2b4h
#define lcdstatus 0e2d3h
org 0030h
main:mov p0,#0ffh
mov p1,#00h
back:mov a,p0
anl a,#0fh
cjne a,#0fh,back
lcall delay
again:mov a,p0
anl a,#0fh
cjne a,0fh,pressed
sjmp again
pressed:acall delay
mov a,p0
anl a,#0fh
cjne a,#0fh,checked
sjmp again
checked:mov p1,# 0eh
mov a,p0
anl a,#0fh
cjne a,#0fh,row1
mov p1, #00001101b
mov a,p0
anl a,#0fh
cjne a,#0fh,row2
mov p1, #00001011b
mov a,p0
anl a,#0fh
cjne a,#0fh,row3
mov p1,#00000111b
mov a,p0
anl a,#0fh
cjne a,#0fh,row4
sjmp again    
row1:mov dptr,# row1codes
sjmp column
row2:mov dptr,# row2codes
sjmp column
row3:mov dptr,# row3codes
sjmp column
row4:mov dptr,# row4codes
column:rrc a
jnc codes
inc dptr
sjmp column
codes:clr a
movc a,@a+dptr
mov r0,a
lcall lcdinit
mov dptr,#disp
hold:clr a
movc a,@a+dptr
lcall lcddat
jz two          
inc dptr
sjmp hold
two:lcall lcdstatus
mov a,r0
lcall lcddat
ljmp main
org 0400h
row1codes:db '0','1','2','3'
row2codes:db '4','5','6','7'
row3codes:db '8','9','a','b'
row4codes:db 'c','d','e','f'
disp:db "key pressed",0
 org 0450h
 delay:mov r0,#0ffh
 loop2:mov r1,#23h
 loop1:djnz r1,loop1
 djnz r0,loop2
 ret
 end
