# https://pdfcpu.io/extract/extract_content

chown ubadm /home/ubadm/test/pdfcpu/cmd/pdfcpu


git clone https://github.com/hhrutter/pdfcpu
git reset --hard 9d476ddd92a
git checkout 9d476ddd92a

cd pdfcpu/cmd/pdfcpu

go build -o pdfcpu
GOOS=js GOARCH=wasm go build -o pdfcpu.wasm

go env

cp /usr/lib/go-1.22/misc/wasm/wasm_exec.html ./
cp /usr/lib/go-1.22/misc/wasm/wasm_exec.js ./
cp /usr/lib/go-1.22/misc/wasm/wasm_exec_node.js ./

---------------------------------------------------------

$ ./pdfcpu validate PDFReference.pdf
validating(mode=relaxed) PDFReference.pdf ...
validation ok

$ ./pdfcpu trim -pages 1 PDFReference.pdf first_page.pdf
pageSelection: 1
trimming PDFReference.pdf ...
writing first_page.pdf ...

# first_page.pdf is a 26KB pdf file

$ node wasm_exec.js pdfcpu.wasm validate PDFReference.pdf
validating(mode=relaxed) PDFReference.pdf ...
validation ok

$ node wasm_exec.js pdfcpu.wasm trim -pages 1 PDFReference.pdf first_page.pdf
pageSelection: 1
trimming PDFReference.pdf ...
writing first_page.pdf ...

./pdfcpu extract -mode content -pages 1 1.pdf ./

---------------------------------------------------------

pdfcpu is a tool for PDF manipulation written in Go. 
	
Usage:
	
	pdfcpu command [arguments]
		
The commands are:

   attachments list, add, remove, extract embedded file attachments
   changeopw   change owner password
   changeupw   change user password
   decrypt     remove password protection
   encrypt     set password protection		
   extract     extract images, fonts, content, pages, metadata
   grid        rearrange pages orimages for enhanced browsing experience
   import      import/convert images
   merge       concatenate 2 or more PDFs
   nup         rearrange pages or images for reduced number of pages
   optimize    optimize PDF by getting rid of redundant page resources
   pages       insert, remove selected pages
   paper       print list of supported paper sizes
   permissions list, add user access permissions
   rotate      rotate pages
   split       split multi-page PDF into several PDFs according to split span
   stamp       add text, image or PDF stamp to selected pages
   trim        create trimmed version with selected pages
   validate    validate PDF against PDF 32000-1:2008 (PDF 1.7)
   version     print version
   watermark   add text, image or PDF watermark to selected pages

   Completion supported for all commands.
   One letter Unix style abbreviations supported for flags.

Use "pdfcpu help [command]" for more information about a command.

---------------------------------------------------------

## Running a command line tool written in Go on browser with WebAssembly

This repo contains code/assets from the [article](article.md)

Files:
```
.
├── article.md           ---  Article explaining the code
├── index.html           ---  Demo UI page
├── worker.js            ---  JS code for Demo UI Page
├── oldindex.html        ---  Code from the article
├── pdfcpu               ---  Linux x86_64 native binary
├── pdfcpu.wasm          ---  Compiled wasm module of pdfcpu
├── pdfcpu.wasm.br       ---  Brotli compression output
├── pdfcpu.wasm.gz       ---  Gzip compression output
├── README.md            ---  This file
├── static_server.js     ---  A simple HTTP static file server in Node.js, with wasm MIME type support for development
├── wasm_exec.js.orig    ---  Original wasm_exec.js from Go v1.12.4 installation (misc/wasm), for diff'ing changes
└── wasm_exec.js         ---  Modified to add Filesystem emulation support in browser with BrowserFS
```

