# Penjelasan Kode - Phi
by. [Moh. Abdul Haq Aulia ](https://github.com/MAHAulia/)

ini merupakan penjelasan singkat implementasi kode dari kode sumber -> [phi](https://github.com/mymyid/phi)

## Fungsi main
Fungsi ini merupakan entri point dari aplikasi yang ditulis menggunakan rust, dimana fungsi ini yang akan dijalankan pertama kali ketika kita menjalankan aplikasi rust baik menggunakan perintah 'Cargo run' atau pun menjalankan file hasil kompilasi dari perintah 'Cargo build'

dalam fungsi ini terdapat beberap proses diantaranya.
1. Proses membaca argument

    Pada baris kodes [213](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L213) merupakan proses membaca argument yang diinput oleh user melalui cli misalkan dalam hal ini perintah 
    ```cli 
    cargo run --release -- --model 2  --prompt "A skier slides down a frictionless slope of height 40m and length 80m. What's the skier speed at the bottom?"
    ```
    dalam hal ini terdapt beberapa argument diantaranya "model" dengan nilai "2" dan "prompt" dengan nilai "A skier slides down a frictionless slope of height 40m and length 80m. What's the skier speed at the bottom?"

2. Menentukan crome_layer

    Pada baris kode [216-222](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L216) membaca argument "tracing" dan menentukan opsi untuk crhome layer

3. Pengecekan argument temparatur dan repeat

    Pada baris kode [234-239](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L234) kita melakukan pengecekan apakah ada inputan untuk beberapa argument diantaranya "temperature", "repeat_penalty", dan "repeat_last_n"

5. Inisiasi Huggingface Hub

    Pada baris kode [245](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L245) kita melakukan inisiasi api untuk terhubung ke huggingface hub untuk kebutuhan proses mendownload model

6. Menentukan model_id

    Pada baris kode [247-263](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L247) kita memanfaatkan pattern matcing yang ada pada rust untuk mentnukan model ini dari inputan argument, dalam hal ini argumenya adalah "model_id". kondisi pengecekanya adalah sebagai berikut.
    1. Jika model_id tidak kosong maka gunakan model_id dari input argument.
    2. Jika kosong maka cek argumen "quantized" yang memiliki type boolean, dan jika nilainya "true" maka gunakan model_id = "lmz/candle-quantized-phi", namun jika tidak maka kita akan melakukan pengecekan lagi dari argument model dengan memanfaatkan pattern matching lagi dan enum yang kita milikis
      1. jika argumen "model" bernilai sama dengan enum WhichModel::V1 atau nilinya adalah "0" maka gunakan "microsoft/phi-1"
      2. jika "model" bernilai "1" maka gunakan "microsoft/phi-1_5"
      3. jika "model" bernilai "2" atau "3" maka gunakan "microsoft/phi-2"
      4. Jika "model" bernilaii "4" atau "5" maka gunakan "lmz/candle-quantized-phi"

7. Menentukan revision

    Pada baris kode [266](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L266) kita akan menentukan revision repository yang akan kita gunakan dari argumen "revision" dimana jika kita memberikan argument "revision" pada perintah kita maka revision yang akan kita gunakan sesuai dengan yang kita inputkan, namun jika tidak maka kita akan melakukan pengecekan dari argument "quantized".
    1. jika nilai argument "quantized" adalah "true" maka revisionnya "main"
    2. selain itu kita akan menggunakan argument "model" untuk melakukan proses penentuan revision 
      1. jika argumen "model" bernilai sama dengan enum WhichModel::V1 atau nilinya adalah "0" maka gunakan "refs/pr/8"
      2. jika "model" bernilai "1" maka gunakan "refs/pr/73"
      3. jika "model" bernilai "3" maka gunakan "834565c23f9b28b96ccbeabe614dd906b6db551a"
      4. Jika "model" bernilaii "2" atau "4" atau "5" maka gunakan "main"
    
    revision ini akan kita gunakan untuk menentukan revisi model yang akan kita gunakan nantinya.

8. Set Repo Huggingface Hub

    Pada baris kode [285](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L285) kita menentukan repository pada huggingface yang akan kita gunakan dari model_id dan revision yang telah kita tentukan sebelumnya

9. Menentukan tokenizer_filename

    Pada baris kode [288-298](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L288) kita akan menentukan nama file tokenizer dari argument tokenizer, jika kita sebelumnya sudah memiliki file tokenizer dalam bentuk "json", maka kita bisa menggunakan argument ini untuk menentukan file tokenizer yang akan digunakan. dalam hal ini kita akan melakukan pengecekan.
    1. jika argument "tokenizer" kita isi, maka kita akan menggunakan nilai dari argument tokenizer ini.
    2. jika tidak maka kita akan melakukan pengecekan dari argument model sebagai berikut.
      1. Jika "model" berinali "0", "1", "2", atau "2" maka gunakan nama file "tokenizer.json", 
      2. jika "model" bernilai "4", atau "5" maka gunakan nama file "tokenizer-puffin-phi-v2.json"
    
10. Menentukan filenames

    Pada baris kode [301-324](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L288) kita akan menentukan nama file dari argument weight_file, dimana jika kita sebelumnya sudah memiliki file weight_file kita bisa menyebutkan lokasinya, dan jika tidak maka kita akan melakukan proses download ke huggingface hub dengan bepgecekan sebeagai berikut.
    1. jik argument "quantized" bernilai "true", maka kita akan melakukan pengecekan pada argument "model" untuk menentukan file mana yang akan kita download
      1. jika argumen "model" bernilai sama dengan enum WhichModel::V1 atau nilinya adalah "0" maka repo.get("model-v1-q4k.gguf")
      2. jika "model" bernilai "1" maka repo.get("model-q4k.gguf")
      3. jika "model" bernilai "2", atau "3" maka repo.get("model-v2-q4k.gguf")
      4. Jika "model" bernilaii "4" maka repo.get("model-puffin-phi-v2-q4k.gguf")
      5. jika "model" bernilai "5" maka repo.get("model-phi-hermes-1_3B-q4k.gguf")
    2. namun jika "quantized" bernilai "false" kita akan melakuan download dengan pengecekan sebagai berikut.
      1. jika "model" bernilai "0" atau "1" maka repo.get("model.safetensors")
      2. jika "model" bernilai "2" atau "3" maka candle_examples::hub_load_safetensors(
                        &repo,
                        "model.safetensors.index.json",
                    )
      3. jika "model" bernilai "4" maka repo.get("model-puffin-phi-v2.safetensors")?
      4. jika "model" bernilai "5" maka repo.get("model-phi-hermes-1_3B.safetensors")

11. Melakukan Tokenizer dari tokenizer_filename yang sudah kita dapatkan sebelumnya

      Pada baris kode [330](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L330) kita melakukan tokenizer dengan memanfaatkan candle-tokenizer dari file yang telah kita miliki

12. Menentukan config

      Pada baris kode [335-341](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L335) kita akan menentukan konfigurasi dari model yang telah kita tentukan, dalam hal ini kita menggunakan pattern matching untuk mengambil konfigurasi sesuai dengan konfigurasi yang telah kita definisikan

13. Menentukan Jenis Perangkat Yang Akan digunakan

      Pada baris kode [344](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L344) kita akan menentukan apakah kita akan menggunakan cpu, atau menggunakan cuda dan lain sebagainya dari argumen yang kita set pada argument "cpu" saat menjalankan aplikasi

14. Melakkan setup Model

      Pada baris kode [347-377](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L347) kita akan menentukan bagaimana model yang akan kita gunakan dari argument "quantized",
      1. jika kita set argumetn "quantized" bernilai "true", kita akan melakukan proses quantized dari file yang telah kita tentukan
      2. jika tidak maka kita akan melakuan proses download dari huggingface hub

15. Menjalankan Prompt

      Pada baris kode [381-400](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L381) pada bagian ini kita akan menjalankan dengan 2 model, mode prompt atau mode multiple choice

      1. Mode Prompt ini akan berjalan ketika kita memberikan argument "prompt" saja dan akan menjalankan pipeline dari pipeline text generation. pada mode ini  aplikasi akan memanfaatkan hugginface text generation pipleine dari model, tokenizer, device dan argument lain yang telah kita set sebelumnya
      2. Mode Mulitple choice ini akan berjalan jika kita memberikan argumetn "mmlu_dir" tanpa argument "prompt", untuk mode ini akan menjalankan proses training dari file csv yang kita sebutkan di argument mmlu_dir, lebih detail lihat secction [Fungsi mmlu](https://github.com/MAHAulia/belajar_huggingface/blob/master/Phi-Doc.md#L109)

## Fungsi Pipeline TextGeneration
fungsi ini terdapat pada implementasi dari struct TextGeneration dimana di dalamnya terdapat 2 fungsi yaitu new dan juga run.     

fungsi "new" ini digunakan untuk melakukan set data pada struct TextGenaration dimana fungsi "run" ini merupakan implementasi dari huggingface pipeline text generation

untuk fungsi run sendiri prosenya sebagai berikut.

1. Pada baris kode [69-86](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L69) fungsi ini melakukan encode ke token dari prompt yang disuplay dan melakukan sedikit validasi data token jika isinya kosong.
2. Pada baris kode [88-118](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L88) akan melakkan pengulangan sebanyak sample_len dimana valuenya secara default adalah 5000 dapat dilihat pada struct "Args". proses ini kurang lebih akan melakuan proses pengulanan yang isinya diantaranya menetukan contenxt, membuat data input tensor kemudian melakuan implementasi data input tadi ke model dan seterusnya sampai pengulangan selesai.

## Fungsi mmlu
Fungsi ini akan menjalankan tugas multiple choice question answer yang dipanggil pada bris code main di baris ke [399](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L399) fungsi ini akan dipanggil dengan cara seperti berikut

```code
mmlu(model, tokenizer, &device, mmlu_dir)
```
fungsi ini menerima 3 parameter yaitu model, tokenizer, device, dan mmludir. dan fungsi ini berjalan sebagai berikut.

1. Fungsi ini akan melakukan looping dari argeumetn "mmlu_dir" yang diberikan.
2. dalam proses loopingnya fungsi ini akan membaca setiap direktori yang disuplay pada argument mmlu_dir
3. untuk setiap iterasinya berjalan sebagai berikut.

    1. Melakukan pembersihan format data

        Pada baris kode [414-320](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L414) fungsi ini akan membaca path yang didapatkan dan melakukan replace tanda "_" menjadi " "

    2. Melakukan validasi extensi

        pada baris kode [421-423](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L421) fungsi ini melakuan pengecekan apakah "path" mengandung tulisan "csv" jika tidak maka prosesnya akan dilanjutkan ke iterasi berikutnya, ini digunakan untuk memastikan path yang digunakan adala path yang berisi file dengan extensi csv

    3. Membuat file reader

        Pada baris kode [427-430](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L427) fungsi ini membuat file reader dari direktory yang telah divalidasi dan disuplay sebelumnya

    4. Membuat Token untuk Pilihan A, B, C, D

        Pada baris kode [433-436](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L433) fungsi ini melkaukan tokenisasi untuk kebutuhan pilihan opsi A, B, C, D dengan memanfaatkan "tokenizer"

    5. Tahap ke 5 ini merupakan proses perulangan dari konten file csv yang dibaca dengan proses sebagai beriut.

        1. Melakukan Validasi data,

            Pada baris kode [439-445](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L439) fungsi ini melakukan validasi data dan menyimpanya pada variable "row", dimana ketika datanya error maka prosesnya akan dilewati, dan jika hasil dari "row" ini panjangnya kurang dari 5 maka prosenya juga dilewati.

        2. Memebuat format "prompt"

            Pada baris kode [448-459](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L448)
            Untuk kebutuhan encoding ke token, fungsi ini akan melakukan generate string dengan format " {theme}.\n{question}\nA. {answer_a}\nB. {answer_b}\nC. {answer_c}\nD. {answer_d}\nAnswer:\n". dimana "theme", "question", "answer_a", "answer_b", "answer_c", "answer_d", dan "answer" didapat dari proses membaca row di baris code [448-453](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L448)

        3. Implementasi prompt ke model

            Pada baris kode [462-480](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L462) fungsi ini melakukan proses encoding dari prompt yang dibuat, lalu membuat data tensor baru pada bais kode [464](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L464). lalu hasil dari data tensor tadi dilakukan proses untuk mendapatkan data vector dari model dan data tensor yang dibuat.

        4. Pengecekan hasil Jawaban

            Pada baris kode [481-495](https://github.com/MAHAulia/belajar_huggingface/blob/15b02a2f77f330c1b8acecc4ba1e0ec58a988408/phi/src/main.rs#L481) fungsi ini mengambil nilai dari data vecotr logist_v dengan token a, b, c, d dan melakukan pencocokan nilai untuk memutuskan jawaban mana yang benar.


## Enum
dalam aplikasi ini terdapat beberapa enum diantaranya
1. Model

    Enum ini digunakan untuk kebutuhan pencocokandalam proses pattern matching
    ```enum Model
    enum Model {
        MixFormer(MixFormer),
        Phi(Phi),
        Quantized(QMixFormer),
    }
    ```

2. WichModel

    Enum ini digunakan untuk kebutuhan pattern matching penentuan jenis model inputan
    ```enum WhichModel
      #[derive(Clone, Copy, Debug, ValueEnum, PartialEq, Eq)]
        enum WhichModel {
            #[value(name = "1")]
            V1,
            #[value(name = "1.5")]
            V1_5,
            #[value(name = "2")]
            V2,
            #[value(name = "2-old")]
            V2Old,
            PuffinPhiV2,
            PhiHermes,
        }
    ```

## Strunct & Implementation
1. Struct TextGeneration dan Implement of Struct TextGenerataion

    1. struct TextGeneration

        struct ini berfungsi untuk menampung type data TextGeneration yang nantinya akan ada implementasi untuk kebutuhan pipeline textgeneration dari huggingface  
        ```struct TextGeneration
          struct TextGeneration {
              model: Model,
              device: Device,
              tokenizer: Tokenizer,
              logits_processor: LogitsProcessor,
              repeat_penalty: f32,
              repeat_last_n: usize,
              verbose_prompt: bool,
          }
        ```

    2. imp TextGeneration

        imp TextGenerationini merupakan impelemntasi dari struct TextGeneration, dimana di dalamnya ada fungsi fungsi yang ada hubunganya dengan pipeline text
        ```imp TextGeneration 
          mpl TextGeneration {
              #[allow(clippy::too_many_arguments)]
              fn new(
                  model: Model,
                  tokenizer: Tokenizer,
                  seed: u64,
                  temp: Option<f64>,
                  top_p: Option<f64>,
                  repeat_penalty: f32,
                  repeat_last_n: usize,
                  verbose_prompt: bool,
                  device: &Device,
              ) -> Self {
                  let logits_processor = LogitsProcessor::new(seed, temp, top_p);
                  Self {
                      model,
                      tokenizer,
                      logits_processor,
                      repeat_penalty,
                      repeat_last_n,
                      verbose_prompt,
                      device: device.clone(),
                  }
              }

              
              fn run(&mut self, prompt: &str, sample_len: usize) -> Result<()> {
                  use std::io::Write;
                  println!("starting the inference loop");
                  let tokens = self.tokenizer.encode(prompt, true).map_err(E::msg)?;
                  if tokens.is_empty() {
                      anyhow::bail!("Empty prompts are not supported in the phi model.")
                  }
                  if self.verbose_prompt {
                      for (token, id) in tokens.get_tokens().iter().zip(tokens.get_ids().iter()) {
                          let token = token.replace('▁', " ").replace("<0x0A>", "\n");
                          println!("{id:7} -> '{token}'");
                      }
                  }
                  let mut tokens = tokens.get_ids().to_vec();
                  let mut generated_tokens = 0usize;
                  let eos_token = match self.tokenizer.get_vocab(true).get("<|endoftext|>") {
                      Some(token) => *token,
                      None => anyhow::bail!("cannot find the endoftext token"),
                  };
                  print!("{prompt}");
                  std::io::stdout().flush()?;
                  let start_gen = std::time::Instant::now();
                  for index in 0..sample_len {
                      let context_size = if index > 0 { 1 } else { tokens.len() };
                      let ctxt = &tokens[tokens.len().saturating_sub(context_size)..];
                      let input = Tensor::new(ctxt, &self.device)?.unsqueeze(0)?;
                      let logits = match &mut self.model {
                          Model::MixFormer(m) => m.forward(&input)?,
                          Model::Phi(m) => m.forward(&input)?,
                          Model::Quantized(m) => m.forward(&input)?,
                      };
                      let logits = logits.squeeze(0)?.to_dtype(DType::F32)?;
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
                      let token = self.tokenizer.decode(&[next_token], true).map_err(E::msg)?;
                      print!("{token}");
                      std::io::stdout().flush()?;
                  }
                  let dt = start_gen.elapsed();
                  println!(
                      "\n{generated_tokens} tokens generated ({:.2} token/s)",
                      generated_tokens as f64 / dt.as_secs_f64(),
                  );
                  Ok(())
              }
          }
        ```

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