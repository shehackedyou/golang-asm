# golang-asm

A mirror of the assembler from the Go compiler, with import paths re-written for the assembler to be functional as a standalone library.

License as per the Go project.

# Status

WIP - while the library builds, I'm still working on the best way to initialize internal structures & use it.

# Example

Demonstrates assembly of a NOP & an ADD instruction on x86-64.

```go

package main

import (
        "fmt"

        asm "github.com/twitchyliquid64/golang-asm"
        "github.com/twitchyliquid64/golang-asm/obj"
        "github.com/twitchyliquid64/golang-asm/obj/x86"
)

func noop(builder *asm.Builder) *obj.Prog {
        prog := builder.NewProg()
        prog.As = x86.ANOPL
        prog.From.Type = obj.TYPE_REG
        prog.From.Reg = x86.REG_AX
        return prog
}

func addImmediateByte(builder *asm.Builder, in int32) *obj.Prog {
        prog := builder.NewProg()
        prog.As = x86.AADDB
        prog.To.Type = obj.TYPE_REG
        prog.To.Reg = x86.REG_AL
        prog.From.Type = obj.TYPE_CONST
        prog.From.Offset = int64(in)
        return prog
}

func main() {
        b, _ := asm.NewBuilder("amd64")
        b.AddInstruction(noop(b))
        b.AddInstruction(addImmediateByte(b, 16))
        bin := b.Assemble()
        fmt.Printf("Bin: %x\n", bin)
}

```
