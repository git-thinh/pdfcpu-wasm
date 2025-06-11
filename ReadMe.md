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