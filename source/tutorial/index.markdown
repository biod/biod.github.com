---
layout: page
comments: false
sharing: true
footer: true

---

<h4>Installing a D compiler</h4>

<p>
The easiest way is to <a href="https://dlang.org/download.html" target="_blank">download the relevant DMD compiler</a> for your platform and follow your
platform specific instructions to install the precompiled binaries. Users on MacOS can use homebrew to install dmd or the ldc compilers.
</p>

<p>
You can use your favourite text editor to create a file hello.d, type the following code,

```D print ready to use
import std.stdio;

void main() {
  writeln("I am ready to use BioD!");
}

```

Compile and run the program

```sh compiling and running a D program with DMD
$ dmd hello.d  //compile it

$ ./hello     // run the compiled program

$ I am ready to use BioD
```

Congratulations, your D compiler is waiting for your instructions.  

</p>

<h4>Using BioD in your project</h4>

<p>
To take advantage of BioD in your project, you can include the library as a dependency using DUB, 
D's package management utility. However if you do not like DUB, you can clone the library into  your existing project directory 
and link to it during compiling step. For example,

<p>

</p>
</p>

<h4>Examples of using BioD</h4>
<ol>
<li>
<h5>Reading a BAM file</h5>

``` D Reading a BAM file 
import std.stdio;

import bio.std.hts.bam.reader;
import bio.std.hts.bam.pileup;

void main() {

 auto bam = new BamReader("my_file.bam");
 auto reads = bam["chr1"][150 .. 160];
 auto pileup = makePileup(reads,false,155, 158);

 foreach (column; pileup) {
        writeln("Reference position: ", column.position);
        writeln("    Coverage: ", column.coverage);
        writeln("    Reads:");

 foreach (read; column.reads) {
    writefln("%30s\t%s\t%.2d\t%s\t%2s/%2s\t%2s/%2s\t%10s\t%s %s", 
      read.name, 
      read.current_base,
      read.current_base_quality,
      read.cigar_operation,
      read.cigar_operation_offset + 1, read.cigar_operation.length,
      read.query_offset + 1, read.sequence.length,
      read.cigarString(),
      read.cigar_before, read.cigar_after);
       }
     }
  }

```
Example output

```sh example output for a single position
Reference position: 155
    Coverage: 11
    Reads:
        EAS221_3:4:30:1452:1563   A       27      35M     35/35   35/35          35M      [] []
        EAS114_45:1:77:1000:1780  A       22      35M     34/35   34/35          35M      [] []
        EAS114_45:4:48:310:473    A       24      35M     34/35   34/35          35M      [] []
        B7_591:2:279:124:41       A       20      36M     33/36   33/36          36M      [] []
        EAS112_32:8:89:254:332    A       26      35M     33/35   33/35          35M      [] []
        B7_597:7:103:731:697      A       25      35M     32/35   32/35          35M      [] []
        EAS139_11:2:71:83:58      A       27      9M       9/ 9    9/35      9M2I24M      [] [2I, 24M]
        EAS192_3:4:63:5:870       A       27      9M       9/ 9    9/35      9M2I24M      [] [2I, 24M]
        EAS139_19:2:29:1822:1881  A       27      7M       7/ 7    7/40      7M2I31M      [] [2I, 31M]
        EAS221_3:2:100:1147:124   G       27      35M      7/35    7/35          35M      [] []
        EAS192_3:8:6:104:118      G       27      35M      3/35    3/35          35M      [] []

```

</li>

<li>

<h5>Reading multiple BAM files</h5>

```
// 
import bio.std.hts.bam.multireader;
import bio.std.hts.bam.read : compareCoordinates;
import bion.std.hts.bam.pileup;

import std.algorithm;
import std.conv;
import std.stdio;

void main(){
 // if the bam files can be merger, they can be transversed simultinously. :)
 auto bam = new MultiBamReader(["../test/data/illu_20_chunk.bam", "../test/data/ion_20_chuck.bam"]);
 auto pileup = makePileup(bam.reads,true,32_000_083,32_000_089);
 
 foreach(column;pileup)
    writeln("Column position: ", column.position);
    writeln("   Ref.base: ", column.reference_base);
    writeln("   Coverage: ", column.coverage);
 }

}
```
Expected output 

```
Column position: 32000083
    Ref.base: G
    Coverage: 23
    GGGGGGGGGGGGGGGGGGGGGGG
 Column position: 32000084
     Ref.base: C
     Coverage: 23
     CCCCCCCCCCCCCCCCCCCCCCC
 Column position: 32000085
     Ref.base: C
     Coverage: 23
     CCCCCCCCCCCCCCCCCCCCCCC
  Column position: 32000086
     Ref.base: C
     Coverage: 24
     CCCCCCCCCCCCGCCCCCCCCCCC
  Column position: 32000087
     Ref.base: C
     Coverage: 24
     CCCCCCCCCCCCCCCCCCCCCCCC
  Column position: 32000088
     Ref.base: T
     Coverage: 24
     TTTTTTTTTTTTTTTTTTTTTTTT

```
  </li>

 <li>
<p>Working with pileup</p>
 </li>

</ol>


