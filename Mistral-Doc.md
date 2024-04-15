# Penjelasan Kode - Mistral
by. [Moh. Abdul Haq Aulia ](https://github.com/MAHAulia/)

ini merupakan penjelasan singkat implementasi kode dari kode sumber -> [mistral](https://github.com/mymyid/mistral)

## Fungsi main
Fungsi ini merupakan entri point dari aplikasi yang ditulis menggunakan rust, dimana fungsi ini yang akan dijalankan pertama kali ketika kita menjalankan aplikasi rust baik menggunakan perintah 'Cargo run' atau pun menjalankan file hasil kompilasi dari perintah 'Cargo build'

dalam fungsi ini terdapat beberap proses diantaranya.
1. Proses membaca argument

    Pada baris kodes [180](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L180) merupakan proses membaca argument yang diinput oleh user melalui cli misalkan dalam hal ini perintah 
    ```cli 
    RUST_BACKTRACE=1 cargo run --features cuda -- --prompt "Here is a sample quick sort implementation in rust " -n 400
    ```
    dalam hal ini terdapt beberapa argument diantaranya "prompt" dengan nilai "the smallest prime is"

2. Menentukan crome_layer

    Pada baris kode [181-187](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L181) membaca argument "tracing" dan menentukan opsi untuk crhome layer

3. Pengecekan argument temparatur dan repeat

    Pada baris kode [195-200](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L195) kita melakukan pengecekan apakah ada inputan untuk beberapa argument diantaranya "temperature", "repeat_penalty", dan "repeat_last_n"

5. Inisiasi Huggingface Hub

    Pada baris kode [203](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L203) kita melakukan inisiasi api untuk terhubung ke huggingface hub untuk kebutuhan proses mendownload model

6. Menentukan Model ID

    Pada baris kode [204](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L204) kita akan menetukan model_id dari argumenyang kita berikan, jika kita tidak mengisi, maka kita akan melakukan pengecekan pada rgumen quantized, 
    1. jika "quantized" bernilai "true" maka gunakan model_id "lmz/candle-mistral"
    2. jika tidak maka gunakan "mistralai/Mistral-7B-v0.1"

7. Set Repo Huggingface Hub

    Pada baris kode [214-218](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L214) kita menentukan repository pada huggingface yang akan kita gunakan dari model_id dan revision yang telah kita tentukan sebelumnya

8. Menentukan tokenizer

    Pada baris kode [219-222](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L219) kita akan menentukan nama file tokenizer dari argument tokenizer, jika kita sebelumnya sudah memiliki file tokenizer dalam bentuk "json", maka kita bisa menggunakan argument ini untuk menentukan file tokenizer yang akan digunakan. dalam hal ini kita akan melakukan pengecekan.
    1. jika argument "tokenizer" kita isi, maka kita akan menggunakan nilai dari argument tokenizer ini.
    2. jika tidak maka kita akan melakukan pengecekan dari argument model maka akan menggunakan "tokenizer.json"

9. Menentukan filenames

    Pada baris kode [223282-235](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L223) kita akan menentukan nama file dari argument weight_file, dimana jika kita sebelumnya sudah memiliki file weight_file kita bisa menyebutkan lokasinya, dan jika tidak maka kita akan melakukan proses download ke huggingface hub dengan bepgecekan sebeagai berikut.
    1. jik argument "quantized" bernilai "true", maka kita akan melakukan melakukan download untuk model "model-q4k.gguf"
    2. namun jika "quantized" bernilai "false" kita akan melakuan download  "model.safetensors.index.json"

10. Melakukan Tokenizer dari tokenizer_filename yang sudah kita dapatkan sebelumnya

       Pada baris kode [237](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L237) kita melakukan tokenizer dengan memanfaatkan candle-tokenizer dari file yang telah kita miliki

11. Menentukan config

      Pada baris kode [240](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L240) kita akan menentukan konfigurasi dari model yang telah kita tentukan dari arguman use_flash_attn, proses pengambilan konfigurasi dari configurasi Mistral config_7v_v0_1

12. Menentukan Jenis Perangkat Yang Akan digunakan

      Pada baris kode [241](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L241) kita akan menentukan apakah kita akan menggunakan cpu, atau menggunakan cuda dan lain sebagainya dari argumen yang kita set pada argument "cpu" saat menjalankan aplikasi

13. Melakkan setup Model

      Pada baris kode [242-257](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L242) kita akan menentukan bagaimana model yang akan kita gunakan dari argument "quantized",
      1. jika kita set argumetn "quantized" bernilai "true", kita akan melakukan proses quantized dari file yang telah kita tentukan
      2. jika tidak maka kita akan melakuan proses download dari huggingface hub

14. Menjalankan Prompt

      Pada baris kode [261-271](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/mistral/src/main.rs#L261) pada proses ini kita memanfaatkan candle textgenration untuk melakukan generate text dari pipeline yang kita jalankan

## Fungsi Pipeline TextGeneration
fungsi ini terdapat pada implementasi dari struct TextGeneration dimana di dalamnya terdapat 2 fungsi yaitu new dan juga run.     

fungsi "new" ini digunakan untuk melakukan set data pada struct TextGenaration dimana fungsi "run" ini merupakan implementasi dari huggingface pipeline text generation

untuk fungsi run sendiri prosenya sebagai berikut.

1. Pada baris kode [72-82](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L72) fungsi ini melakukan encode ke token dari prompt yang disuplay.

2. Pada baris kode [85-112](https://github.com/MAHAulia/belajar_huggingface/blob/ad93008f7f57d8c08fb6d95a35e1d10138bc4342/rwkv/src/main.rs#L85) akan melakkan pengulangan sebanyak sample_len dimana valuenya secara default adalah 5000 dapat dilihat pada struct "Args". proses ini kurang lebih akan melakuan transformasi menggunakan candle-transformer sebanyak sample_len untuk menghasilkan token

## Enum & Implemetasi
dalam aplikasi ini terdapat beberapa enum dan implementasinya diantaranya
1. Model

    Enum ini digunakan untuk kebutuhan pencocokandalam proses pattern matching
    ```enum Model
    enum Model {
        Mistral(Mistral),
        Quantized(QMistral),
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
                        tokenizer: TokenOutputStream::new(tokenizer),
                        logits_processor,
                        repeat_penalty,
                        repeat_last_n,
                        device: device.clone(),
                    }
                }

                fn run(&mut self, prompt: &str, sample_len: usize) -> Result<()> {
                    use std::io::Write;
                    self.tokenizer.clear();
                    let mut tokens = self
                        .tokenizer
                        .tokenizer()
                        .encode(prompt, true)
                        .map_err(E::msg)?
                        .get_ids()
                        .to_vec();
                    for &t in tokens.iter() {
                        if let Some(t) = self.tokenizer.next_token(t)? {
                            print!("{t}")
                        }
                    }
                    std::io::stdout().flush()?;

                    let mut generated_tokens = 0usize;
                    let eos_token = match self.tokenizer.get_token("</s>") {
                        Some(token) => token,
                        None => anyhow::bail!("cannot find the </s> token"),
                    };
                    let start_gen = std::time::Instant::now();
                    for index in 0..sample_len {
                        let context_size = if index > 0 { 1 } else { tokens.len() };
                        let start_pos = tokens.len().saturating_sub(context_size);
                        let ctxt = &tokens[start_pos..];
                        let input = Tensor::new(ctxt, &self.device)?.unsqueeze(0)?;
                        let logits = match &mut self.model {
                            Model::Mistral(m) => m.forward(&input, start_pos)?,
                            Model::Quantized(m) => m.forward(&input, start_pos)?,
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
                        if next_token == eos_token {
                            break;
                        }
                        if let Some(t) = self.tokenizer.next_token(next_token)? {
                            print!("{t}");
                            std::io::stdout().flush()?;
                        }
                    }
                    let dt = start_gen.elapsed();
                    if let Some(rest) = self.tokenizer.decode_rest().map_err(E::msg)? {
                        print!("{rest}");
                    }
                    std::io::stdout().flush()?;
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