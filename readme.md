open/seek/read/write any binary file in ProDOS filesystem.<br>
runs from any directory on floppy or hard disk.<br>
searches the file system for the requested file, can even look inside subdirectories.<br>
can simulate DOS 3.3 RWTS (including format) to allow disk images on the hard disk, and multiple sides in the same file.<br>
can read sparse files, and read and write tree files<br>
<br>
usage:<br>
<br>
jsr init ;one-time call to unhook ProDOS, detect drive type, and relocate code if needed<br>
<br>
;open and read bytes from a file without address override or writing enabled<br>
lda #<file_to_read<br>
sta namlo<br>
lda #>file_to_read<br>
sta namhi<br>
lda #6 ;bytes<br>
sta sizelo<br>
lda #0<br>
sta sizehi<br>
jsr opendir ;open and read file into memory at its load address<br>
<br>
;write to an open file with address override<br>
lda #<file_to_write<br>
sta namlo<br>
lda #>file_to_write<br>
sta namhi<br>
lda #0<br>
sta sizelo<br>
lda #>bytes_to_write<br>
sta sizehi<br>
lda #<place_to_write<br>
sta adrlo<br>
lda #>place_to_write<br>
sta adrhi<br>
lda #cmdwrite<br>
sta reqcmd<br>
jsr rdwrpart ;write bytes from memory to specified load address<br>
<br>
<br>
format of request name is Pascal-style (length, text):<br>
e.g. !raw 5, "MYDIR" or !raw 6, "MYFILE" or !raw 14, "QKUMBAROXURSOX"
