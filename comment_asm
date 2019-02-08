#!/bin/sh
true + /; exec gawk -f "$0"; exit; / {}
# for arm asm
# REQUIRED: gawk
{
  const_description_dict["ldp"]="load two registers from successive memory (e.g. pop from stack pointer)"
  const_description_dict["stp"]="store two registers to successive memory (e.g. push to stack pointer)"

  # NOTE: 意味を把握しきれていない
  const_description_dict["adrp"]="Address of 4KB page at a PC-relative offset."

  # NOTE: 簡単な命令なので，不要かも
  const_description_dict["bl"]="branch with link (e.g. subroutine call)"
  const_description_dict["bne"]="branch != (not equal)"
  const_description_dict["beq"]="branch == (equal)"
  const_description_dict["bmi"]="branch < 0 (minus)"
  const_description_dict["bpl"]="branch > 0 (plus)"
  const_description_dict["bvs"]="branch overflow"
  const_description_dict["bvc"]="branch not overflow"
  const_description_dict["bgt"]="branch > (signed)"
  const_description_dict["blt"]="branch < (signed)"
  const_description_dict["bge"]="branch >= (signed)"
  const_description_dict["ble"]="branch <= (signed)"
  const_description_dict["str"]="store"
  const_description_dict["ldr"]="load"
  const_description_dict["ret"]="return"
}
match($0, /\.file[ \t]([0-9]*) "(.*)"$/, m) {
  fileno=m[1]
  filepath=m[2]
  files[fileno]=filepath
}

{
  line=$0
  if(is_skip_line(line)) {
    lines[NR]=line
    next
  }
  if($1 !~ /^\./ && $1 !~ /^[0-9a-zA-Z_]*:/) {
    args=""
    # NOTE: その行に空白が文字列として含まれている場合にその表示が崩れる
    for(i=2;i<=NF;i++) {args=args""sprintf(" %+4s", $i)}
    line=sprintf("\t%-6s%-40s", $1, args)
  }
  lines[NR]=line
}

END {
  for(i=1;i<=NR;i++) {
    line=lines[i]
    if(!is_skip_line(line)) {
      comment=replace_line(line" # ")
      line=add_line_to_comment(line, comment)
    }
    printf "%s\n", line;
  }
  exit exit_code
}

function is_skip_line(line) {
  return line ~ /^[ \t]*#/ || line ~ /#[ \t]*$/
}

function replace_line(line) {
  comment=loc_line(line)
  if(comment!="") return comment
  comment=const_description_line(line)
  if(comment!="") return comment
  return comment
}

function add_line_to_comment(line, comment) {
  if(comment!="") {
    return sprintf("%s # %s #", line, comment)
  }
  gsub(" *$", "", line)
  return sprintf("%s", line)
}

function const_description_line(line) {
  for (key in const_description_dict) {
    val = const_description_dict[key]
    if (match(line, "\t"key, m)) {
      return val
    }
  }
  return ""
}

function loc_line(line) {
  if (match(line, /^\t\.loc[ \t]([0-9]*) ([0-9]*)/, m)) {
    fileno=m[1]
    lineno=m[2]
    filepath=files[fileno]
    command="sed -n "lineno"p "filepath
    command | getline ret_line
    close(command)
    comment=sprintf("%-80s '%s:%d'", ret_line, filepath, lineno)
    return comment
  }
  return ""
}
