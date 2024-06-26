摘抄自：[GitHub - ctf-wiki/ctf-wiki](https://github.com/ctf-wiki/ctf-wiki)

# ELF 文件

> 本部分内容来源于 ELF 1.2 标准，内容经过一定的修改与整理，主要参考文献如下
> 
> 1. ELF 文件格式分析，北京大学，滕启明
>     
> 2. ELF-摧毁圣诞
>     

## 简介

ELF （Executable and Linkable Format）文件，也就是在 Linux 中的目标文件，主要有以下三种类型

- 可重定位文件（Relocatable File），包含由编译器生成的代码以及数据。链接器会将它与其它目标文件链接起来从而创建可执行文件或者共享目标文件。在 Linux 系统中，这种文件的后缀一般为 `.o` 。
    
- 可执行文件（Executable File），就是我们通常在 Linux 中执行的程序。
    

- 共享目标文件（Shared Object File），包含代码和数据，这种文件是我们所称的库文件，一般以 `.so` 结尾。一般情况下，它有以下两种使用情景：
    
    - 链接器（Link eDitor, ld）可能会处理它和其它可重定位文件以及共享目标文件，生成另外一个目标文件。
        
    - 动态链接器（Dynamic Linker）将它与可执行文件以及其它共享目标组合在一起生成进程镜像。
        

> 关于Link eDitor的命名，[https://en.wikipedia.org/wiki/GNU_linker](https://en.wikipedia.org/wiki/GNU_linker)

目标文件由汇编器和链接器创建，是文本程序的二进制形式，可以直接在处理器上运行。那些需要虚拟机才能够执行的程序(Java)不属于这一范围。

这里我们主要关注于 ELF 的文件格式。

### 文件格式

目标文件既会参与程序链接又会参与程序执行。出于方便性和效率考虑，根据过程的不同，目标文件格式提供了其内容的两种并行视图，如下

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/object_file_format-206c64.png)

首先，我们来**关注一下链接视图**。

文件开始处是 ELF 头部（ **ELF Header**），它给出了整个文件的组织情况。

如果程序头部表（Program Header Table）存在的话，它会告诉系统如何创建进程。用于生成进程的目标文件必须具有程序头部表，但是重定位文件不需要这个表。

节区部分包含在链接视图中要使用的大部分信息：指令、数据、符号表、重定位信息等等。

节区头部表（Section Header Table）包含了描述文件节区的信息，每个节区在表中都有一个表项，会给出节区名称、节区大小等信息。用于链接的目标文件必须有节区头部表，其它目标文件则无所谓，可以有，也可以没有。

这里给出一个关于链接视图比较形象的展示

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/elf-layout-496746.png)

对于**执行视图**来说，其主要的不同点在于没有了section，而有了多个segment。其实这里的 segment 大都是来源于链接视图中的 section。

注意:

> 尽管图中是按照 ELF 头，程序头部表，节区，节区头部表的顺序排列的。但实际上除了 ELF 头部表以外，其它部分都没有严格的顺序。

### 数据形式

ELF 文件格式支持 8 位/32 位体系结构。当然，这种格式是可以扩展的，也可以支持更小的或者更大位数的处理器架构。因此，目标文件会包含一些控制数据，这部分数据表明了目标文件所使用的架构，这也使得它可以被通用的方式来识别和解释。目标文件中的其它数据采用目的处理器的格式进行编码，与在何种机器上创建没有关系。这里其实想表明的意思目标文件可以进行交叉编译，我们可以在 x86 平台生成 arm 平台的可执行代码。

目标文件中的所有数据结构都遵从“自然”大小和对齐规则。如下

|名称|长度|对齐方式|用途|
|---|---|---|---|
|Elf32_Addr|4|4|无符号程序地址|
|Elf32_Half|2|2|无符号半整型|
|Elf32_Off|4|4|无符号文件偏移|
|Elf32_Sword|4|4|有符号大整型|
|Elf32_Word|4|4|无符号大整型|
|unsigned char|1|1|无符号小整型|

如果必要，数据结构可以包含显式地补齐来确保 4 字节对象按 4 字节对齐，强制数据结构的大小是 4 的整数倍等等。数据同样适用是对齐的。因此，包含一个 Elf32_Addr 类型成员的结构体会在文件中的 4 字节边界处对齐。

为了具有可移植性，ELF 文件不使用位域。

### 字符表示

待。

**注：在下面的介绍中，我们以 32 位为主进行介绍。**

## ELF Header

ELF Header 描述了 ELF 文件的概要信息，利用这个数据结构可以索引到 ELF 文件的全部信息，数据结构如下：

```c
#define EI_NIDENT   16  
​  
typedef struct {  
    unsigned char   e_ident[EI_NIDENT];  
    ELF32_Half  e_type;  
    ELF32_Half  e_machine;  
    ELF32_Word  e_version;  
    ELF32_Addr  e_entry;  
    ELF32_Off   e_phoff;  
    ELF32_Off   e_shoff;  
    ELF32_Word  e_flags;  
    ELF32_Half  e_ehsize;  
    ELF32_Half  e_phentsize;  
    ELF32_Half  e_phnum;  
    ELF32_Half  e_shentsize;  
    ELF32_Half  e_shnum;  
    ELF32_Half  e_shstrndx;  
} Elf32_Ehdr;
```

其中每个成员都是 e 开头的，它们应该都是 ELF 的缩写。每个成员具体的说明如下。

### e_ident

正如之前所说，ELF 提供了一个目标文件框架，以便于支持多种处理器，多种编码格式的机器。该变量给出了用于解码和解释文件中与机器无关的数据的方式。这个数组对于不同的下标的含义如下

|宏名称|下标|目的|
|---|---|---|
|EI_MAG0|0|文件标识|
|EI_MAG1|1|文件标识|
|EI_MAG2|2|文件标识|
|EI_MAG3|3|文件标识|
|EI_CLASS|4|文件类|
|EI_DATA|5|数据编码|
|EI_VERSION|6|文件版本|
|EI_PAD|7|补齐字节开始处|

其中，

`e_ident[EI_MAG0]` 到 `e_ident[EI_MAG3]`，即文件的头4个字节，被称作“魔数”，标识该文件是一个ELF目标文件。**至于开头为什么是0x7f，并没有仔细去查过**。

|名称|值|位置|
|---|---|---|
|ELFMAG0|0x7f|e_ident[EI_MAG0]|
|ELFMAG1|‘E’|e_ident[EI_MAG1]|
|ELFMAG2|‘L’|e_ident[EI_MAG2]|
|ELFMAG3|‘F’|e_ident[EI_MAG3]|

`e_ident[EI_CLASS]` 为 `e_ident[EI_MAG3]`的下一个字节，标识文件的类型或容量。

|名称|值|意义|
|---|---|---|
|ELFCLASSNONE|0|无效类型|
|ELFCLASS32|1|32位文件|
|ELFCLASS64|2|64位文件|

ELF 文件的设计使得它可以在多种字节长度的机器之间移植，而不需要强制规定机器的最长字节长度和最短字节长度。`ELFCLASS32`类型支持文件大小和虚拟地址空间上限为 4GB 的机器；它使用上述定义中的基本类型。

`ELFCLASS64` 类型用于 64 位架构。

`e_ident[EI_DATA]`字节给出了目标文件中的特定处理器数据的编码方式。下面是目前已定义的编码：

|名称|值|意义|
|---|---|---|
|ELFDATANONE|0|无效数据编码|
|ELFDATA2LSB|1|小端|
|ELFDATA2MSB|2|大端|

其它值被保留，在未来必要时将被赋予新的编码。

文件数据编码方式表明了文件内容的解析方式。正如之前所述，`ELFCLASS32`类型文件使用了具有1，2 和 4 字节的变量类型。对于已定义的不同的编码方式，其表示如下所示，其中字节号在左上角。

`ELFDATA2LSB`编码使用补码，最低有效位（Least Significant Byte）占用最低地址。

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/elfdata2lsb-eae3bf.png)

`ELFDATA2MSB`编码使用补码，最高有效位（Most Significant Byte）占用最低地址。

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/elfdata2msb-ca7901.png)

`e_ident[EI_DATA]` 给出了 ELF 头的版本号。目前这个值必须是`EV_CURRENT`，即之前已经给出的`e_version`。

`e_ident[EI_PAD]` 给出了 `e_ident` 中未使用字节的开始地址。这些字节被保留并置为0；处理目标文件的程序应该忽略它们。如果之后这些字节被使用，EI_PAD的值就会改变。

### e_type

`e_type` 标识目标文件类型。

|名称|值|意义|
|---|---|---|
|ET_NONE|0|无文件类型|
|ET_REL|1|可重定位文件|
|ET_EXEC|2|可执行文件|
|ET_DYN|3|共享目标文件|
|ET_CORE|4|核心转储文件|
|ET_LOPROC|0xff00|处理器指定下限|
|ET_HIPROC|0xffff|处理器指定上限|

虽然核心转储文件的内容没有被详细说明，但 `ET_CORE` 还是被保留用于标志此类文件。从 `ET_LOPROC` 到 `ET_HIPROC` (包括边界)被保留用于处理器指定的场景。其它值在未来必要时可被赋予新的目标文件类型。

### e_machine

这一项指定了当前文件可以运行的机器架构。

|名称|值|意义|
|---|---|---|
|EM_NONE|0|无机器类型|
|EM_M32|1|AT&T WE 32100|
|EM_SPARC|2|SPARC|
|EM_386|3|Intel 80386|
|EM_68K|4|Motorola 68000|
|EM_88K|5|Motorola 88000|
|EM_860|7|Intel 80860|
|EM_MIPS|8|MIPS RS3000|

其中 EM 应该是 `ELF Machine` 的简写。

其它值被在未来必要时用于新的机器。 此外，特定处理器的ELF名称使用机器名称来进行区分，一般标志会有个前缀`EF_` （ELF Flag）。例如，在`EM_XYZ`机器上名叫 `WIDGET` 的标志将被称为 `EF_XYZ_WIDGET`。

### e_version

标识目标文件的版本。

|名称|值|意义|
|---|---|---|
|EV_NONE|0|无效版本|
|EV_CURRENT|1|当前版本|

1 表示初始文件格式；未来扩展新的版本的时候(extensions)将使用更大的数字。虽然在上面值`EV_CURRENT`为1，但是为了反映当前版本号，它可能会改变，**比如ELF到现在也就是1.2版本。**

### e_entry

这一项为系统转交控制权给 ELF 中相应代码的虚拟地址。如果没有相关的入口项，则这一项为0。

### e_phoff

这一项给出**程序头部表**在文件中的字节偏移（**Program Header table OFFset**）。如果文件中没有程序头部表，则为0。

### e_shoff

这一项给出**节头表**在文件中的字节偏移（ **Section Header table OFFset** ）。如果文件中没有节头表，则为0。

### e_flags

这一项给出文件中与特定处理器相关的标志，这些标志命名格式为`EF_machine_flag`。

### e_ehsize

这一项给出 ELF 文件头部的字节长度（ELF Header Size）。

### e_phentsize

这一项给出程序头部表中每个表项的字节长度（**Program Header ENTry SIZE**）。每个表项的大小相同。

### e_phnum

这一项给出程序头部表的项数（ **Program Header entry NUMber** ）。因此，`e_phnum` 与 `e_phentsize` 的乘积即为程序头部表的字节长度。如果文件中没有程序头部表，则该项值为0。

### e_shentsize

这一项给出节头的字节长度（**Section Header ENTry SIZE**）。一个节头是节头表中的一项；节头表中所有项占据的空间大小相同。

### e_shnum

这一项给出节头表中的项数（**Section Header NUMber**）。因此， `e_shnum` 与 `e_shentsize` 的乘积即为节头表的字节长度。如果文件中没有节头表，则该项值为0。

### e_shstrndx

这一项给出节头表中与节名字符串表相关的表项的索引值（**Section Header table InDeX related with section name STRing table**）。如果文件中没有节名字符串表，则该项值为`SHN_UNDEF`。关于细节的介绍，请参考后面的“节”和“字符串表”部分。

## Program Header Table

### 概述

Program Header Table 是一个结构体数组，每一个元素的类型是 `Elf32_Phdr`，描述了一个段或者其它系统在准备程序执行时所需要的信息。其中，ELF 头中的 `e_phentsize` 和 `e_phnum` 指定了该数组每个元素的大小以及元素个数。一个目标文件的段包含一个或者多个节。**程序的头部只有对于可执行文件和共享目标文件有意义。**

可以说，Program Header Table 就是专门为 ELF 文件运行时中的段所准备的。

`Elf32_Phdr` 的数据结构如下

typedef struct {  
    ELF32_Word  p_type;  
    ELF32_Off   p_offset;  
    ELF32_Addr  p_vaddr;  
    ELF32_Addr  p_paddr;  
    ELF32_Word  p_filesz;  
    ELF32_Word  p_memsz;  
    ELF32_Word  p_flags;  
    ELF32_Word  p_align;  
} Elf32_Phdr;

每个字段的说明如下

|字段|说明|
|---|---|
|p_type|该字段为段的类型，或者表明了该结构的相关信息。|
|p_offset|该字段给出了从文件开始到该段开头的第一个字节的偏移。|
|p_vaddr|该字段给出了该段第一个字节在内存中的虚拟地址。|
|p_paddr|该字段仅用于物理地址寻址相关的系统中， 由于“System V”忽略了应用程序的物理寻址，可执行文件和共享目标文件的该项内容并未被限定。|
|p_filesz|该字段给出了文件镜像中该段的大小，可能为0。|
|p_memsz|该字段给出了内存镜像中该段的大小，可能为0。|
|p_flags|该字段给出了与段相关的标记。|
|p_align|可加载的程序的段的 p_vaddr 以及 p_offset 的大小必须是 page 的整数倍。该成员给出了段在文件以及内存中的对齐方式。如果该值为 0 或 1 的话，表示不需要对齐。除此之外，p_align 应该是 2 的整数指数次方，并且 p_vaddr 与 p_offset 在模 p_align 的意义下，应该相等。|

### 段类型

可执行文件中的段类型如下

|名字|取值|说明|
|---|---|---|
|PT_NULL|0|表明段未使用，其结构中其他成员都是未定义的。|
|PT_LOAD|1|此类型段为一个可加载的段，大小由 p_filesz 和 p_memsz 描述。文件中的字节被映射到相应内存段开始处。如果 p_memsz 大于 p_filesz，“剩余”的字节都要被置为0。p_filesz 不能大于 p_memsz。可加载的段在程序头部中按照 p_vaddr 的升序排列。|
|PT_DYNAMIC|2|此类型段给出动态链接信息。|
|PT_INTERP|3|此类型段给出了一个以 NULL 结尾的字符串的位置和长度，该字符串将被当作解释器调用。这种段类型仅对可执行文件有意义（也可能出现在共享目标文件中）。此外，这种段在一个文件中最多出现一次。而且这种类型的段存在的话，它必须在所有可加载段项的前面。|
|PT_NOTE|4|此类型段给出附加信息的位置和大小。|
|PT_SHLIB|5|该段类型被保留，不过语义未指定。而且，包含这种类型的段的程序不符合ABI标准。|
|PT_PHDR|6|该段类型的数组元素如果存在的话，则给出了程序头部表自身的大小和位置，既包括在文件中也包括在内存中的信息。此类型的段在文件中最多出现一次。**此外，只有程序头部表是程序的内存映像的一部分时，它才会出现**。如果此类型段存在，则必须在所有可加载段项目的前面。|
|PT_LOPROC~PT_HIPROC|0x70000000 ~0x7fffffff|此范围的类型保留给处理器专用语义。|

### 基地址-Base Address

程序头部的虚拟地址可能并不是程序内存镜像中实际的虚拟地址。通常来说，可执行程序都会包含绝对地址的代码。为了使得程序可以正常执行，段必须在相应的虚拟地址处。另一方面，共享目标文件通常来说包含与地址无关的代码。这可以使得共享目标文件可以被多个进程加载，同时保持程序执行的正确性。尽管系统会为不同的进程选择不同的虚拟地址，但是它仍然保留段的相对地址，**因为地址无关代码使用段之间的相对地址来进行寻址，内存中的虚拟地址之间的差必须与文件中的虚拟地址之间的差相匹配**。内存中任何段的虚拟地址与文件中对应的虚拟地址之间的差值对于任何一个可执行文件或共享对象来说是一个单一常量值。这个差值就是基地址，基地址的一个用途就是在动态链接期间重新定位程序。

可执行文件或者共享目标文件的基地址是在执行过程中由以下三个数值计算的

- 虚拟内存加载地址
    
- 最大页面大小
    
- 程序可加载段的最低虚拟地址
    

要计算基地址，首先要确定可加载段中 p_vaddr 最小的内存虚拟地址，之后把该内存虚拟地址缩小为与之最近的最大页面的整数倍即是基地址。根据要加载到内存中的文件的类型，内存地址可能与 p_vaddr 相同也可能不同。

### 段权限-p_flags

被系统加载到内存中的程序至少有一个可加载的段。当系统为可加载的段创建内存镜像时，它会按照 p_flags 将段设置为对应的权限。可能的段权限位有

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/segment_flag_bits-703b38.png)

其中，所有在 PF_MASKPROC 中的比特位都是被保留用于与处理器相关的语义信息。

如果一个权限位被设置为 0，这种类型的段是不可访问的。实际的内存权限取决于相应的内存管理单元，不同的系统可能操作方式不一样。尽管所有的权限组合都是可以的，但是系统一般会授予比请求更多的权限。在任何情况下，除非明确说明，一个段不会有写权限。下面给出了所有的可能组合。

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/segment-permission-b8f750.png)

例如，一般来说，.text 段一般具有读和执行权限，但是不会有写权限。数据段一般具有写，读，以及执行权限。

### 段内容

一个段可能包括一到多个节区，但是这并不会影响程序的加载。尽管如此，我们也必须需要各种各样的数据来使得程序可以执行以及动态链接等等。下面会给出一般情况下的段的内容。对于不同的段来说，它的节的顺序以及所包含的节的个数有所不同。此外，与处理相关的约束可能会改变对应的段的结构。

如下所示，代码段只包含只读的指令以及数据。当然这个例子并没有给出所有的可能的段。

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/text_segment-e7c058.png)

数据段包含可写的数据以及以及指令，通常来说，包含以下内容

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/data_segment-75235b.png)

程序头部的 PT_DYNAMIC 类型的元素指向 .dynamic 节。其中，got 表和 plt 表包含与地址无关的代码相关信息。尽管在这里给出的例子中，plt 节出现在代码段，但是对于不同的处理器来说，可能会有所变动。

.bss 节的类型为 SHT_NOBITS，这表明它在 ELF 文件中不占用空间，但是它却占用可执行文件的内存镜像的空间。通常情况下，没有被初始化的数据在段的尾部，因此，`p_memsz` 才会比 `p_filesz` 大。

注意：

- 不同的段可能会有所重合，即不同的段包含相同的节。
    

## Section Header Table

其实这个数据结构是在 ELF 文件的尾部（ **为什么要放在文件尾部呢？？** ），但是为了讲解方便，这里将这个表放在这里进行讲解。

该结构用于定位 ELF 文件中的每个节区的具体位置。

首先，ELF头中的 `e_shoff` 项给出了从文件开头到节头表位置的字节偏移。`e_shnum` 告诉了我们节头表包含的项数；`e_shentsize` 给出了每一项的字节大小。

其次，节头表是一个数组，每个数组的元素的类型是 `ELF32_Shdr` ，每一个元素都描述了一个节区的概要内容。

### ELF32_Shdr

每个节区头部可以用下面的数据结构进行描述：

typedef struct {  
    ELF32_Word  sh_name;  
    ELF32_Word  sh_type;  
    ELF32_Word  sh_flags;  
    ELF32_Addr  sh_addr;  
    ELF32_Off   sh_offset;  
    ELF32_Word  sh_size;  
    ELF32_Word  sh_link;  
    ELF32_Word  sh_info;  
    ELF32_Word  sh_addralign;  
    ELF32_Word  sh_entsize;  
} Elf32_Shdr;

每个字段的含义如下

|成员|说明|
|---|---|
|sh_name|节名称，是节区头字符串表节区中（Section Header String Table Section）的索引，因此该字段实际是一个数值。在字符串表中的具体内容是以 NULL 结尾的字符串。|
|sh_type|根据节的内容和语义进行分类，具体的类型下面会介绍。|
|sh_flags|每一比特代表不同的标志，描述节是否可写，可执行，需要分配内存等属性。|
|sh_addr|如果节区将出现在进程的内存映像中，此成员给出节区的第一个字节应该在进程镜像中的位置。否则，此字段为 0。|
|sh_offset|给出节区的第一个字节与文件开始处之间的偏移。SHT_NOBITS 类型的节区不占用文件的空间，因此其 sh_offset 成员给出的是概念性的偏移。|
|sh_size|此成员给出节区的字节大小。除非节区的类型是 SHT_NOBITS ，否则该节占用文件中的 sh_size 字节。类型为SHT_NOBITS 的节区长度可能非零，不过却不占用文件中的空间。|
|sh_link|此成员给出节区头部表索引链接，其具体的解释依赖于节区类型。|
|sh_info|此成员给出附加信息，其解释依赖于节区类型。|
|sh_addralign|某些节区的地址需要对齐。例如，如果一个节区有一个 doubleword 类型的变量，那么系统必须保证整个节区按双字对齐。也就是说，$sh_addr \% sh_addralign$=0。目前它仅允许为 0，以及 2 的正整数幂数。 0 和 1 表示没有对齐约束。|
|sh_entsize|某些节区中存在具有固定大小的表项的表，如符号表。对于这类节区，该成员给出每个表项的字节大小。反之，此成员取值为0。|

正如之前所说，索引为零（SHN_UNDEF）的节区头也存在，此索引标记的是未定义的节区引用。这一项的信息如下

|字段名称|取值|说明|
|---|---|---|
|sh_name|0|无名称|
|sh_type|SHT_NULL|限制|
|sh_flags|0|无标志|
|sh_addr|0|无地址|
|sh_offset|0|无文件偏移|
|sh_size|0|无大小|
|sh_link|SHN_UNDEF|无链接信息|
|sh_info|0|无辅助信息|
|sh_addralign|0|无对齐要求|
|sh_entsize|0|无表项|

### 特殊下标

节头表中比较特殊的几个下标如下

|名称|值|含义|
|---|---|---|
|SHN_UNDEF|0|标志未定义的，丢失的，不相关的或者其它没有意义的节引用。例如，与节号SHN_UNDEF相关的“定义”的符号就是一个未定义符号。**注：虽然0号索引被保留用于未定义值，节头表仍然包含索引0的项。也就是说，如果ELF头的e_shnum为6，那么索引应该为0~5。更加详细的内容在后面会说明。**|
|SHN_LORESERVE|0xff00|保留索引值范围的下界。|
|SHN_LOPROC|0xff00|处理器相关的下界|
|SHN_HIPROC|0xff1f|处理器相关的上界|
|SHN_ABS|0xfff1|相关引用的绝对值。例如与节号SHN_ABS相关的符号拥有绝对值，它们不受重定位的影响|
|SHN_COMMON|0xfff2|这一节区相定义的符号是通用符号，例如FORTRAN COMMON，C语言中未分配的外部变量。|
|SHN_HIRESERVE|0xffff|保留索引值范围的上界。|

**系统保留在`SHN_LORESERVE`到`SHN_HIRESERVE`之间(包含边界)的索引值，这些值不在节头表中引用。也就是说，节头表不包含保留索引项。没特别理解。**

### 部分节头字段

#### sh_type

节类型目前有下列可选范围，其中 SHT 是**Section Header Table** 的简写。

|名称|取值|说明|
|---|---|---|
|SHT_NULL|0|该类型节区是非活动的，这种类型的节头中的其它成员取值无意义。|
|SHT_PROGBITS|1|该类型节区包含程序定义的信息，它的格式和含义都由程序来决定。|
|SHT_SYMTAB|2|该类型节区包含一个符号表（**SYMbol TABle**）。目前目标文件对每种类型的节区都只 能包含一个，不过这个限制将来可能发生变化。 一般，SHT_SYMTAB 节区提供用于链接编辑（指 ld 而言） 的符号，尽管也可用来实现动态链接。|
|SHT_STRTAB|3|该类型节区包含字符串表（ **STRing TABle** ）。|
|SHT_RELA|4|该类型节区包含显式指定位数的重定位项（ **RELocation entry with Addends** ），例如，32 位目标文件中的 Elf32_Rela 类型。此外，目标文件可能拥有多个重定位节区。|
|SHT_HASH|5|该类型节区包含符号哈希表（ **HASH table** ）。|
|SHT_DYNAMIC|6|该类型节区包含动态链接的信息（ **DYNAMIC linking** ）。|
|SHT_NOTE|7|该类型节区包含以某种方式标记文件的信息（**NOTE**）。|
|SHT_NOBITS|8|该类型节区不占用文件的空间，其它方面和SHT_PROGBITS相似。尽管该类型节区不包含任何字节，其对应的节头成员sh_offset 中还是会包含概念性的文件偏移。|
|SHT_REL|9|该类型节区包含重定位表项（**RELocation entry without Addends**），不过并没有指定位数。例如，32位目标文件中的 Elf32_rel 类型。目标文件中可以拥有多个重定位节区。|
|SHT_SHLIB|10|该类型此节区被保留，不过其语义尚未被定义。|
|SHT_DYNSYM|11|作为一个完整的符号表，它可能包含很多对动态链接而言不必 要的符号。因此，目标文件也可以包含一个 SHT_DYNSYM 节区，其中保存动态链接符号的一个最小集合，以节省空间。|
|SHT_LOPROC|0X70000000|此值指定保留给处理器专用语义的下界（ **LOw PROCessor-specific semantics** ）。|
|SHT_HIPROC|OX7FFFFFFF|此值指定保留给处理器专用语义的上界（ **HIgh PROCessor-specific semantics** ）。|
|SHT_LOUSER|0X80000000|此值指定保留给应用程序的索引下界。|
|SHT_HIUSER|0X8FFFFFFF|此值指定保留给应用程序的索引上界。|

#### sh_flags

节头中 `sh_flags` 字段的每一个比特位都可以给出其相应的标记信息，其定义了对应的节区的内容是否可以被修改、被执行等信息。如果一个标志位被设置，则该位取值为1，未定义的位都为0。目前已定义值如下，其他值保留。

|名称|值|说明|
|---|---|---|
|SHF_WRITE|0x1|这种节包含了进程运行过程中可以被写的数据。|
|SHF_ALLOC|0x2|这种节在进程运行时占用内存。对于不占用目标文件的内存镜像空间的某些控制节，该属性处于关闭状态(off)。|
|SHF_EXECINSTR|0x4|这种节包含可执行的机器指令（**EXECutable INSTRuction**）。|
|SHF_MASKPROC|0xf0000000|所有在这个掩码中的比特位用于特定处理器语义。|

#### sh_link & sh_info

当节区类型的不同的时候，sh_link 和 sh_info 也会具有不同的含义。

|sh_type|sh_link|sh_info|
|---|---|---|
|SHT_DYNAMIC|节区中使用的字符串表的节头索引|0|
|SHT_HASH|此哈希表所使用的符号表的节头索引|0|
|SHT_REL/SHT_RELA|与符号表相关的节头索引|重定位应用到的节的节头索引|
|SHT_SYMTAB/SHT_DYNSYM|操作系统特定信息，Linux 中的 ELF 文件中该项指向符号表中符号所对应的字符串节区在 Section Header Table 中的偏移。|操作系统特定信息|
|other|`SHN_UNDEF`|0|

## 例子

### HelloWorld

C语言源代码
```c
#include<stdio.h>

int main(){
	printf("Hello World");
	return 0;
}
```

elf信息
```sh
$ readelf a.out -h
ELF 头：
  Magic：  7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  类别:                              ELF64
  数据:                              2 补码，小端序 (little endian)
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI 版本:                          0
  类型:                              DYN (Position-Independent Executable file)
  系统架构:                          Advanced Micro Devices X86-64
  版本:                              0x1
  入口点地址：              0x1040
  程序头起点：              64 (bytes into file)
  Start of section headers:          13496 (bytes into file)
  标志：             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         13
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```

TODO

## `Simple.out`

这里给出一个 elf 文件比较经典的例子。

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/ELF-Walkthrough-0c965e.png)
## 参考文献

-   https://blogs.oracle.com/ali/gnu-hash-elf-sections
-   https://bbs.pediy.com/thread-204642.htm