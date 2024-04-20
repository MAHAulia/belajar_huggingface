# Belajar Hugging Face

## Hugging Face Candle
Candle adalah toolkit yang dirancang untuk memfasilitasi proses pelatihan dan penyetelan model machine learning, terutama model bahasa alami, menggunakan Transformers. ->
[doucmentation](https://huggingface.github.io/candle/index.html)

"Hugging Face" adalah perusahaan yang mengembangkan perangkat lunak untuk pemrosesan bahasa alami dan pembelajaran mesin. "Candle-Phi" adalah salah satu proyek yang mereka kembangkan.

CANDLE (Continuous Active Learning for Digital Evidence) adalah sebuah inisiatif yang bertujuan untuk mengembangkan alat-alat otomatis untuk menganalisis dan memahami bukti digital dalam kasus hukum, seperti dokumen, catatan medis, dan lainnya.

## Other Resource

### phi

source -> [phi](https://github.com/mymyid/phi)

### Documentation
[Phi - Documentation](Phi-Doc.md)

#### Run Code
```phi
cargo run --release -- --model 2  --prompt "A skier slides down a frictionless slope of height 40m and length 80m. What's the skier speed at the bottom?"
```
<img src="docs/phi.png" alt="phi result command" width="80%">

### Performance
<table>
  <tr>
    <td>Waktu Generatsi Kalimat</td>
    <td>:</td>
    <td>???</td>
  </tr>
  <tr>
    <td>Akurasi</td>
    <td>:</td>
    <td>???</td>
  </tr>
  <tr>
    <td>Kebutuhan Kompuasi</td>
    <td>:</td>
    <td>???</td>
  </tr>
</table>
<br/>
<hr/>
<br/>
### rwkv

source -> [rwkv](https://github.com/mymyid/rwkv)

### Documentation
[Rwkv - Documentation](Rwkv-Doc.md)

#### Run Code
```rwkv
cargo run -- --prompt "the smallest prime is"
```
<img src="docs/rwkv.png" alt="rwkv result command" width="80%">

### Performance
<table>
  <tr>
    <td>Waktu Generatsi Kalimat</td>
    <td>:</td>
    <td>???</td>
  </tr>
  <tr>
    <td>Akurasi</td>
    <td>:</td>
    <td>???</td>
  </tr>
  <tr>
    <td>Kebutuhan Kompuasi</td>
    <td>:</td>
    <td>???</td>
  </tr>
</table>

<br/>
<hr/>
<br/>

### mistral

source -> [mistral](https://github.com/mymyid/mistral)

### Documentation
[Mistral - Documentation](Mistral-Doc.md)

#### Run Code
```mistral
RUST_BACKTRACE=1 cargo run --features cuda -- --prompt "Here is a sample quick sort implementation in rust " -n 400
```
<img src="docs/mistral.png" alt="mistral result command" width="80%">

### Performance
<table>
  <tr>
    <td>Waktu Generatsi Kalimat</td>
    <td>:</td>
    <td>???</td>
  </tr>
  <tr>
    <td>Akurasi</td>
    <td>:</td>
    <td>???</td>
  </tr>
  <tr>
    <td>Kebutuhan Kompuasi</td>
    <td>:</td>
    <td>???</td>
  </tr>
</table>

<br/>
<hr/>
<br/>

## Troubleshooting
Proses troubleshooting selama menajalankan aplikasi memanfaatkan informasi saat compile error ketika aplikasi rust dijalankan.