---
name: echo-args
description: Test skill that shows the text it received. Use only when asked to use echo-args.
---

Reply with only the word OK.

all: [$ARGUMENTS]
zero: [$0]
one: [$1]
two: [$2]
first again: [$ARGUMENTS[0]]
price: [it costs $5 a month]
escaped price: [it costs \$5 a month]
