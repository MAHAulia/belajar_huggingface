# Penjelasan Kode - Rwkv
by. [Moh. Abdul Haq Aulia ](https://github.com/MAHAulia/)

ini merupakan penjelasan singkat implementasi kode dari kode sumber -> [rwkv](https://github.com/mymyid/rwkv)

## Fungsi main
Fungsi ini merupakan entri point dari aplikasi yang ditulis menggunakan rust, dimana fungsi ini yang akan dijalankan pertama kali ketika kita menjalankan aplikasi rust baik menggunakan perintah 'Cargo run' atau pun menjalankan file hasil kompilasi dari perintah 'Cargo build'

dalam fungsi ini terdapat beberap proses diantaranya.
1. Proses membaca argument

    Pada baris kodes [241](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L241) merupakan proses membaca argument yang diinput oleh user melalui cli misalkan dalam hal ini perintah 
    ```cli 
    cargo run -- --prompt "the smallest prime is"
    ```
    dalam hal ini terdapt beberapa argument diantaranya "prompt" dengan nilai "the smallest prime is"

2. Menentukan crome_layer

    Pada baris kode [242-248](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L242) membaca argument "tracing" dan menentukan opsi untuk crhome layer

3. Pengecekan argument temparatur dan repeat

    Pada baris kode [254-261](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L256) kita melakukan pengecekan apakah ada inputan untuk beberapa argument diantaranya "temperature", "repeat_penalty", dan "repeat_last_n"

5. Inisiasi Huggingface Hub

    Pada baris kode [264](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L264) kita melakukan inisiasi api untuk terhubung ke huggingface hub untuk kebutuhan proses mendownload model

6. Set Repo Huggingface Hub

    Pada baris kode [265-271](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L265) kita menentukan repository pada huggingface yang akan kita gunakan dari model_id dan revision yang telah kita tentukan sebelumnya

7. Menentukan tokenizer

    Pada baris kode [272-277](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L272) kita akan menentukan nama file tokenizer dari argument tokenizer, jika kita sebelumnya sudah memiliki file tokenizer dalam bentuk "json", maka kita bisa menggunakan argument ini untuk menentukan file tokenizer yang akan digunakan. dalam hal ini kita akan melakukan pengecekan.
    1. jika argument "tokenizer" kita isi, maka kita akan menggunakan nilai dari argument tokenizer ini.
    2. jika tidak maka kita akan melakukan pengecekan dari argument model maka akan menggunakan model "lmz/candle-rwkv" dan file "rwkv_vocab_v20230424.json"
    
8. Menetukan config_filename

    Pada baris kode [278-281](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L278) kita menentukan config_filename dari argumen config_filename dan jika tidak ada nilainya maka config file diambil dari repo dengan nama config.json

9. Menentukan filenames

    Pada baris kode [282-310](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L282) kita akan menentukan nama file dari argument weight_file, dimana jika kita sebelumnya sudah memiliki file weight_file kita bisa menyebutkan lokasinya, dan jika tidak maka kita akan melakukan proses download ke huggingface hub dengan bepgecekan sebeagai berikut.
    1. jik argument "quantized" bernilai "true", maka kita akan melakukan pengecekan pada argument "model" untuk menentukan file mana yang akan kita download
      1. jika argumen "model" bernilai sama dengan enum WhichModel::V1 atau nilinya adalah "1" maka gunakan model "lmz/candle-rwkv" dan ambil file "world1b5-q4k.gguf"
      2. jika "model" bernilai "2" maka gunakan model "lmz/candle-rwkv" dan ambil file "world3b-q4k.gguf"
      3. jika "model" bernilai "3", maka gunakan model "lmz/candle-rwkv" dan ambil file "eagle7b-q4k.gguf"
      4. Jika "model" bernilaii "4" maka gunakan model "lmz/candle-rwkv" dan ambil file "rwkv-6-world-1b6-q4k.gguf"
    2. namun jika "quantized" bernilai "false" kita akan melakuan download dengan pengecekan sebagai berikut.
      1. jika "model" bernilai "1" gunakan model "model.safetensors"
      2. jika "model" bernilai "3" ambil "rwkv-6-world-1b6.safetensors"

10. Melakukan Tokenizer dari tokenizer_filename yang sudah kita dapatkan sebelumnya

       Pada baris kode [312](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L312) kita melakukan tokenizer dengan memanfaatkan candle-tokenizer dari file yang telah kita miliki

11. Menentukan config

      Pada baris kode [315](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L315) kita akan menentukan konfigurasi dari model yang telah kita tentukan, dalam hal ini kita menggunakan pattern matching untuk mengambil konfigurasi sesuai dengan konfigurasi yang telah kita definisikan

12. Menentukan Jenis Perangkat Yang Akan digunakan

      Pada baris kode [316](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L316) kita akan menentukan apakah kita akan menggunakan cpu, atau menggunakan cuda dan lain sebagainya dari argumen yang kita set pada argument "cpu" saat menjalankan aplikasi

13. Melakkan setup Model

      Pada baris kode [317-331](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L317) kita akan menentukan bagaimana model yang akan kita gunakan dari argument "quantized",
      1. jika kita set argumetn "quantized" bernilai "true", kita akan melakukan proses quantized dari file yang telah kita tentukan
      2. jika tidak maka kita akan melakuan proses download dari huggingface hub

14. Menjalankan Prompt

      Pada baris kode [334-345](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L334) pada proses ini kita memanfaatkan candle textgenration untuk melakukan generate text dari pipeline yang kita jalankan

## Fungsi Device
Fungsi ini digunakan untuk kebutuhan menentukan jenis perangkat apa yang akan digunakan, apakah cpu, cuda, metal, atau yang lain

## Fungsi Pipeline TextGeneration
fungsi ini terdapat pada implementasi dari struct TextGeneration dimana di dalamnya terdapat 2 fungsi yaitu new dan juga run.     

fungsi "new" ini digunakan untuk melakukan set data pada struct TextGenaration dimana fungsi "run" ini merupakan implementasi dari huggingface pipeline text generation

untuk fungsi run sendiri prosenya sebagai berikut.

1. Pada baris kode [72-82](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L72) fungsi ini melakukan encode ke token dari prompt yang disuplay.

2. Pada baris kode [85-112](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L85) akan melakkan pengulangan sebanyak sample_len dimana valuenya secara default adalah 5000 dapat dilihat pada struct "Args". proses ini kurang lebih akan melakuan transformasi menggunakan candle-transformer sebanyak sample_len untuk menghasilkan token

## Constanta
1. Const EOS_TOKEN_ID

    Constanta ini digunakan untuk membatasin maksimal looping implementasi text generation
    ``` const EOS_TOKEN_ID
    const EOS_TOKEN_ID: u32 = 261;
    ```

## Enum & Implemetasi
dalam aplikasi ini terdapat beberapa enum dan implementasinya diantaranya
1. Model

    Enum ini digunakan untuk kebutuhan pencocokandalam proses pattern matching
    ```enum Model
    enum Model {
        M5(M5),
        Q5(Q5),
        M6(M6),
        Q6(Q6),
    }
    ```

2. imp Model
    implementasi ini untuk menentukan forward dari model yang ditenukan
    ```imp Model
    impl Model {
        fn forward(&self, xs: &Tensor, state: &mut State) -> candle_core::Result<Tensor> {
            match self {
                Self::M5(m) => m.forward(xs, state),
                Self::Q5(m) => m.forward(xs, state),
                Self::M6(m) => m.forward(xs, state),
                Self::Q6(m) => m.forward(xs, state),
            }
        }
    }
    ```
3. Which
    enum ini diugnakan untuk kebutuhan pengklasifikasian jenis model rwkv yang akan digunakan
    ```enum Which
        enum Which {
            Eagle7b,
            World1b5,
            World3b,
            World6_1b6,
        }
    ```

4. Implementasi Which
    
    implementasi ini digunakan untuk kebutuhan penentuan model_id dan juga revision yang akan diguanakan dari model rwkv

    ``` imp Which
        impl std::fmt::Display for Which {
            fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
                write!(f, "{:?}", self)
            }
        }

        impl Which {
            fn model_id(&self) -> &'static str {
                match self {
                    Self::Eagle7b => "RWKV/v5-Eagle-7B-HF",
                    Self::World1b5 => "RWKV/rwkv-5-world-1b5",
                    Self::World3b => "RWKV/rwkv-5-world-3b",
                    Self::World6_1b6 => "paperfun/rwkv",
                }
            }

            fn revision(&self) -> &'static str {
                match self {
                    Self::Eagle7b => "refs/pr/1",
                    Self::World1b5 | Self::World3b => "refs/pr/2",
                    Self::World6_1b6 => "main",
                }
            }
        }
    ```
## Strunct & Implementation
1. Struct TextGeneration dan Implement of Struct TextGenerataion

    1. struct TextGeneration

        struct ini berfungsi untuk menampung type data TextGeneration yang nantinya akan ada implementasi untuk kebutuhan pipeline textgeneration dari huggingface  
        ```struct TextGeneration
            struct TextGeneration {
                model: Model,
                config: Config,
                device: Device,
                tokenizer: Tokenizer,
                logits_processor: LogitsProcessor,
                repeat_penalty: f32,
                repeat_last_n: usize,
            }
        ```

    2. imp TextGeneration

        imp TextGenerationini merupakan impelemntasi dari struct TextGeneration, dimana di dalamnya ada fungsi fungsi yang ada hubunganya dengan pipeline text
        ```imp TextGeneration 
            impl TextGeneration {
                #[allow(clippy::too_many_arguments)]
                fn new(
                    model: Model,
                    config: Config,
                    tokenizer: Tokenizer,
                    seed: u64,
                    temp: Option<f64>,
                    top_p: Option<f64>,
                    repeat_penalty: f32,
                    repeat_last_n: usize,
                    device: &Device,
                ) -> Self {
                    let logits_processor = LogitsProcessor::new(seed, temp, top_p);
                    Self {
                        model,
                        config,
                        tokenizer,
                        logits_processor,
                        repeat_penalty,
                        repeat_last_n,
                        device: device.clone(),
                    }
                }

                fn run(&mut self, prompt: &str, sample_len: usize) -> Result<()> {
                    use std::io::Write;
                    let mut tokens = self.tokenizer.encode(prompt)?;
                    let mut generated_tokens = 0usize;
                    let mut state = State::new(1, &self.config, &self.device)?;
                    let mut next_logits = None;
                    for &t in tokens.iter() {
                        let input = Tensor::new(&[[t]], &self.device)?;
                        let logits = self.model.forward(&input, &mut state)?;
                        next_logits = Some(logits);
                        print!("{}", self.tokenizer.decode(&[t])?)
                    }
                    std::io::stdout().flush()?;

                    let start_gen = std::time::Instant::now();
                    for _ in 0..sample_len {
                        let logits = match next_logits.as_ref() {
                            Some(logits) => logits,
                            None => anyhow::bail!("cannot work on an empty prompt"),
                        };
                        let logits = logits.squeeze(0)?.squeeze(0)?.to_dtype(DType::F32)?;
                        let logits = if self.repeat_penalty == 1. {
                            logits
                        } else {
                            let start_at = tokens.len().saturating_sub(self.repeat_last_n);
                            candle_transformers::utils::apply_repeat_penalty(
                                &logits,
                                self.repeat_penalty,
                                &tokens[start_at..],
                            )?
                        };
                        let next_token = self.logits_processor.sample(&logits)?;
                        tokens.push(next_token);
                        generated_tokens += 1;
                        if next_token == EOS_TOKEN_ID || next_token == 0 {
                            break;
                        }
                        print!("{}", self.tokenizer.decode(&[next_token])?);
                        std::io::stdout().flush()?;

                        let input = Tensor::new(&[[next_token]], &self.device)?;
                        next_logits = Some(self.model.forward(&input, &mut state)?)
                    }
                    let dt = start_gen.elapsed();
                    println!(
                        "\n{generated_tokens} tokens generated ({:.2} token/s)",
                        generated_tokens as f64 / dt.as_secs_f64(),
                    );
                    Ok(())
                }
            }
        ````
2. Struct Args
    Struct ini digunakan untuk melakan parsing data inputan argument pada cli
    ``` struct Args
        #[derive(Parser, Debug)]
    #[command(author, version, about, long_about = None)]
    struct Args {
        /// Run on CPU rather than on GPU.
        #[arg(long)]
        cpu: bool,

        /// Enable tracing (generates a trace-timestamp.json file).
        #[arg(long)]
        tracing: bool,

        /// Display the token for the specified prompt.
        #[arg(long)]
        verbose_prompt: bool,

        #[arg(long)]
        prompt: Option<String>,

        #[arg(long)]
        mmlu_dir: Option<String>,

        /// The temperature used to generate samples.
        #[arg(long)]
        temperature: Option<f64>,

        /// Nucleus sampling probability cutoff.
        #[arg(long)]
        top_p: Option<f64>,

        /// The seed to use when generating random samples.
        #[arg(long, default_value_t = 299792458)]
        seed: u64,

        /// The length of the sample to generate (in tokens).
        #[arg(long, short = 'n', default_value_t = 5000)]
        sample_len: usize,

        #[arg(long)]
        model_id: Option<String>,

        #[arg(long, default_value = "2")]
        model: WhichModel,

        #[arg(long)]
        revision: Option<String>,

        #[arg(long)]
        weight_file: Option<String>,

        #[arg(long)]
        tokenizer: Option<String>,

        #[arg(long)]
        quantized: bool,

        /// Penalty to be applied for repeating tokens, 1. means no penalty.
        #[arg(long, default_value_t = 1.1)]
        repeat_penalty: f32,

        /// The context size to consider for the repeat penalty.
        #[arg(long, default_value_t = 64)]
        repeat_last_n: usize,
    }
    ```