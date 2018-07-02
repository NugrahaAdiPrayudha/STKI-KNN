#STKI-KNN
#Repository Proyek Sistem Temu Kembali Informasi
package stemming;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.DataInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.io.PrintWriter;
import java.text.Normalizer;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Scanner;
import java.util.SortedSet;
import java.util.TreeMap;
import java.util.TreeSet;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.swing.JFileChooser;
import javax.swing.text.EditorKit;
import org.apache.tika.exception.TikaException;
import org.apache.tika.metadata.Metadata;
import org.apache.tika.parser.ParseContext;
import org.apache.tika.parser.pdf.PDFParser;
import org.apache.tika.sax.BodyContentHandler;
import static org.springframework.util.WeakReferenceMonitor.monitor;
import org.tartarus.snowball.ext.porterStemmer;
import org.xml.sax.SAXException;

/**
 *
 * @author nickname
 */
public class NewJFrame extends javax.swing.JFrame {

    String s1, path, tweet, temp = "";
    
    int batas = 2147483647;
    //my sql stopwords+google stopword
    String[] stopwords = {"a's", "do", "able", "about", "above", "according", "accordingly", "across", "actually", "after", "afterwards",
        "again", "against", "ain't", "all", "allow", "allows", "almost", "alone", "along", "already", "also", "although",
        "always", "am", "among", "amongst", "an", "and", "another", "any", "anybody", "anyhow", "anyone", "anything",
        "anyway", "anyways", "anywhere", "apart", "appear", "appreciate", "appropriate", "are", "aren't", "around", "as",
        "aside", "ask", "asking", "associated", "at", "available", "away", "awfully", "be", "became", "because", "become",
        "becomes", "becoming", "been", "before", "beforehand", "behind", "being", "believe", "belowbeside", "besides", "best",
        "better", "", "between", "beyond", "both", "brief", "but", "by", "c'mon", "c's", "came", "can", "can't", "cannot", "cant",
        "cause", "causes", "certain", "certainly", "changes", "clearly", "co", "com", "come", "comes", "concerning", "consequently",
        "consider", "considering", "contain", "containing", "contains", "corresponding", "could", "couldn't", "course", "currently",
        "definitely", "described", "despite", "did", "didn't", "different", "does", "doesn't", "doing", "don't", "done",
        "down", "downwards", "during", "each", "edu", "eg", "eight", "either", "else", "elsewhere", "enough", "entirely", "especially",
        "et", "etc", "even", "ever", "every", "everybody", "everyone", "everything", "everywhere", "ex", "exactly", "example", "except", "far",
        "few", "fifth", "first", "five", "followed", "following", "follows", "for", "former", "formerly", "forth", "four", "from", "further", "furthermore",
        "get", "gets", "getting", "given", "gives", "go", "goes", "going", "gone", "got", "gotten", "greetings", "had", "hadn't", "happens", "hardly",
        "has", "hasn't", "have", "haven't", "having", "he", "he's", "hello", "help", "hence", "her", "here", "here's", "hereafter", "hereby", "herein",
        "hereupon", "hers", "herself", "hi", "him", "himself", "his", "hither", "hopefully", "how", "howbeit", "however", "i'd", "i'll", "i'm", "i've",
        "ie", "if", "ignored", "immediate", "in", "inasmuch", "inc", "indeed", "indicate", "indicated", "indicates", "inner", "insofar", "instead", "into", "inward",
        "is", "isn't", "it", "it'd", "it'll", "it's", "its", "itself", "just", "keep", "keeps", "kept", "know", "known", "knows", "last", "lately",
        "later", "latter", "latterly", "least", "less", "lest", "let", "let's", "like", "liked", "likely", "little", "look", "looking", "looks", "ltd",
        "mainly", "many", "may", "maybe", "me", "mean", "meanwhile", "merely", "might", "more", "moreover", "most", "mostly", "much", "must", "my", "myself", "name", "namely",
        "nd", "near", "nearly", "necessary", "need", "needs", "neither", "never", "nevertheless", "new", "next", "nine", "no", "nobody", "non", "none", "noone",
        "nor", "normally", "not", "nothing", "novel", "now", "nowhere", "obviously", "of", "off", "often", "oh", "ok", "okay", "old", "on", "once", "one", "ones", "only",
        "onto", "or", "other", "others", "otherwise", "ought", "our", "ours", "ourselves", "out", "outside", "over", "overall", "own", "particular", "particularly", "per", "perhaps",
        "placed", "please", "plus", "possible", "presumably", "probably", "provides", "que", "quite", "qv", "rather", "rd", "re", "really", "reasonably", "regarding",
        "regardless", "regards", "relatively", "respectively", "right", "said", "same", "saw", "say", "saying", "says", "second", "secondly", "see", "seeing", "seem", "seemed", "seeming", "seems",
        "seen", "self", "selves", "sensible", "sent", "serious", "seriously", "seven", "several", "shall", "she", "should", "shouldn't", "since", "six", "so", "some",
        "somebody", "somehow", "someone", "something", "sometime", "sometimes", "somewhat", "somewhere", "soon", "sorry", "specified", "specify", "specifying", "still",
        "sub", "such", "sup", "sure", "t's", "take", "taken", "tell", "tends", "th", "than", "thank", "thanks", "thanx", "that", "that's", "thats", "the", "their", "theirs",
        "them", "themselves", "then", "thence", "there", "there's", "thereafter", "thereby", "therefore", "therein", "theres", "thereupon", "these", "they", "they'd",
        "they'll", "they're", "they've", "think", "third", "this", "thorough", "thoroughly", "those", "though", "three", "through", "throughout", "thru", "thus", "to",
        "together", "too", "took", "toward", "towards", "tried", "tries", "truly", "try", "trying", "twice", "two", "un", "under", "unfortunately", "unless", "unlikely",
        "until", "unto", "up", "upon", "us", "use", "used", "useful", "uses", "using", "usually", "value", "various", "very", "via", "viz", "vs", "want", "wants", "was", "wasn't",
        "way", "we", "we'd", "we'll", "we're", "we've", "welcome", "well", "went", "were", "weren't", "what", "what's", "whatever", "when", "whence", "whenever", "where", "where's",
        "whereafter", "whereas", "whereby", "wherein", "whereupon", "wherever", "whether", "which", "while", "whither", "who", "who's", "whoever", "whole", "whom", "whose", "why",
        "will", "willing", "wish", "with", "within", "without", "won't", "wonder", "would", "wouldn't", "yes", "yet", "you", "you'd", "you'll", "you're", "you've", "your", "yours", "yourself", "yourselves", "zero",
        "i", "a", "about", "an", "are", "as", "at", "be", "by", "com", "for", "from", "how", "in", "is", "it", "of", "on", "or", "thatthe", "this", "to", "was", "what", "when", "where", "who", "will", "with", "the", "www"};
    ArrayList<String> wordsList = new ArrayList<String>();

    public NewJFrame() {
        initComponents();
        proses.setVisible(false);
    }
    
    public class coba {
        //Array untuk pengecekan stop word.
        private ArrayList<String> alExtStopWords = new ArrayList<String>();
        // Fungsi sorting TreeMap berdasarkan value.
        <K, V extends Comparable<? super V>> SortedSet<Map.Entry<K, V>> entriesSortedByValues(Map<K, V> map) {
            SortedSet<Map.Entry<K, V>> sortedEntries = new TreeSet<Map.Entry<K, V>>(
                    new Comparator<Map.Entry<K, V>>() {
                @Override
                public int compare(Map.Entry<K, V> e1, Map.Entry<K, V> e2) {
                    int res = e1.getValue().compareTo(e2.getValue());
                    return res != 0 ? res : 1;
                }
            }
            );
            sortedEntries.addAll(map.entrySet());
            return sortedEntries;
        }
        //Snippet dari program edu.upi.cs.tweetmining.TFIDF untuk memasukkan data stopwords ke array alExtStopWords 

        private void loadExtStopWords(String inputExtStopWords) {
            try {
                FileInputStream fstream = new FileInputStream(inputExtStopWords);
                DataInputStream in = new DataInputStream(fstream);
                BufferedReader br = new BufferedReader(new InputStreamReader(in));
                String strLine;
                int cc = 0;
                while ((strLine = br.readLine()) != null) {
                    alExtStopWords.add(strLine);
                }
                br.close();
                in.close();
            } catch (Exception e) {
                System.out.println(e.toString());
            }
        }

        public void process(String fileInput, boolean denganStat) {
            String namaFile = fileInput.substring(0, fileInput.indexOf("."));
            int totalTerms = 0;
            int totalDoc;
            // mulai load stopwords ke arrayExtStopWords.
//        loadExtStopWords(extStopWord);
            //
            ArrayList<HashMap<String, Integer>> arrTweets = new ArrayList<HashMap<String, Integer>>();
            ArrayList<HashMap<String, Double>> arrTFIDF = new ArrayList<HashMap<String, Double>>();
            HashMap<String, Integer> docFreq = new HashMap<String, Integer>();
            TreeMap<String, Double> tfIDF = new TreeMap<String, Double>();
            try {
                FileInputStream fstream = new FileInputStream(fileInput);
                DataInputStream in = new DataInputStream(fstream);
                BufferedReader br = new BufferedReader(new InputStreamReader(in));
                System.out.println("Reading " + fileInput);
                // HITUNG TERM FREQUENCY
                // Membaca file input
                // Mencari jumlah tf tiap term per baris
                String strLine;
                Integer tfreq;
                while ((strLine = br.readLine()) != null) {
                    HashMap<String, Integer> termFreq = new HashMap<String, Integer>();
                    String docn = strLine.substring(0, strLine.length());
                    Scanner sc = new Scanner(docn);
                    while (sc.hasNext()) {
                        String term = sc.next();

//                    if (!term.equalsIgnoreCase("indosat")) { //Skip keyword indosat, karena ada di setiap tweet.
                        tfreq = termFreq.get(term); //Ambil value
                        termFreq.put(term, (tfreq == null) ? 1 : tfreq + 1); //Jika value masih kosong, isi 1. Jika 1, increment.
                        totalTerms++;
//                    }
                    }
                    sc.close();
                    arrTweets.add(termFreq);//Simpan termFreq.
                }
                br.close();
                // Selesai membaca dataset.
                // arrTweet berisi HashMap termFreq, tiap termFreq adalah representasi dokumen/tweet, berisi jumlah tf dari masing2 term.
                // HITUNG DOCUMENT FREQUENCY
                // Iterasi arrTweets, untuk menghitung df.
                // Menghitung jumlah dokumen yang mengandung term.
                // docFreq.put("awan",7)
                // Artinya term "awan", ditemukan di 7 dokumen/tweet
                Iterator iterArray = arrTweets.iterator();
                while (iterArray.hasNext()) {
                    HashMap perTweet = (HashMap) iterArray.next();

                    Iterator iterEach = perTweet.keySet().iterator();
                    while (iterEach.hasNext()) {
                        String eachW = (String) iterEach.next();
                        if (alExtStopWords.contains(eachW)) { //Kalau ada di stopword, DF = 0.
                            docFreq.put(eachW, 0);
                        } else {
                            Integer dfreq = docFreq.get(eachW);
                            docFreq.put(eachW, (dfreq == null) ? 1 : dfreq + 1);
                        }
                    }
                }
                // Selesai menghitung DF tiap term
                // HashMap docFreq berisi key= term, value= document frequency
                // HITUNG IDF dan TFIDF
                // arrTweets sekali lagi di iterasi
                // untuk menghitung nilai IDF lalu sekaligus dihitung TF*IDF nya
                // di tiap dokumen nilai TF*IDF per term dihitung, dan disimpan di HashMap valTFIDF
                // lalu valTFIDF ini dikumpulkan di arrTFIDF,\
                Iterator iterTF = arrTweets.iterator();
                Double idf, tfidf;
                totalDoc = arrTweets.size();
                while (iterTF.hasNext()) {
                    HashMap<String, Double> valTFIDF = new HashMap<String, Double>();
                    HashMap perTweet = (HashMap) iterTF.next();
                    Iterator iterEach = perTweet.keySet().iterator();
                    while (iterEach.hasNext()) {
                        String aTerm = (String) iterEach.next(); //ambil term yang akan diproses
                        Integer dfreq = docFreq.get(aTerm); //ambil nilai DF dari term yang akan diproses
                        if (dfreq > 1) {
                            Integer cfreq = (Integer) perTweet.get(aTerm); // ambil nilai tf dari aTerm
                            idf = Math.log(totalDoc / dfreq);
                            tfidf = cfreq * idf;
                            valTFIDF.put(aTerm, tfidf);
                            //System.out.println("TFIDF("+aTerm+")= "+cfreq+" * "+"log("+totDoc+"/"+dfreq+") = "+ tfidf+" , ");
                        }
                    }
                    arrTFIDF.add(valTFIDF); //Selesai olah satu perTweet, simpan HashMap valTFIDF ke arrTFIDF
                }
                // Selesai hitung IDF dan TF*IDF
                // arrTFIDF berisi nilai tfidf tiap term per dokumen, yaitu valTFIDF

                // Tulis hasil hitung TF*IDF ke file output namafile_tfidf.txt
                BufferedWriter writeTFIDF = new BufferedWriter(new FileWriter((namaFile + "_tfidf.txt"), true));
                Iterator iterValTFIDF = arrTFIDF.iterator();
                while (iterValTFIDF.hasNext()) {
                    HashMap perTweet = (HashMap) iterValTFIDF.next();
                    //System.out.println(perTweet.toString());
                    Iterator iterEach = perTweet.keySet().iterator();
                    while (iterEach.hasNext()) {
                        String aTerm = (String) iterEach.next();
                        Double valTFIDF = (Double) perTweet.get(aTerm);
                        writeTFIDF.write(aTerm + " = " + valTFIDF + "; ");
                        writeTFIDF.newLine();
                        //System.out.print(aTerm+"="+valTFIDF+"; ");
                    }
                    //System.out.println("__");
                    //writeTFIDF.newLine();
                }
                writeTFIDF.close();
                // Hitung rata-rata bobot TFIDF term, jika denganStat= true
                if (denganStat) {
                    // HITUNG jumlah rata2 TFIDF tiap term
                    for (String word : docFreq.keySet()) {
                        Integer dfreq = docFreq.get(word);
                        if (dfreq > 1) { //hanya hitung term yang muncul di lebih dari satu dokumen 
                            //System.out.println("Collecting term: "+word+" df= "+dfreq);
                            Double tfIDFstat = 0.0; // Inisiasi nilai tfIDFstat, digunakan untuk akumulasi
                            int cc = 0;
                            Iterator iterTFIDF = arrTFIDF.iterator();
                            while (iterTFIDF.hasNext()) {
                                HashMap val = (HashMap) iterTFIDF.next();

                                if (val.containsKey(word)) {
                                    for (Object t : val.keySet()) {
                                        if (t.toString().equals(word)) {
                                            cc++;
                                            tfIDFstat = tfIDFstat + (Double) val.get(word); //akumulasi nilai tfidf suatu term di seluruh dokumen
                                        }
                                    }
                                }
                            }
                            //System.out.println("Counted="+cc+"   tfIDFstats="+tfIDFstat);
                            Double tfIDFtot = tfIDFstat / cc;                                 //HITUNG RATA-RATA
                            //System.out.println("tfidf("+word+")="+tfIDFtot);
                            tfIDF.put(word, tfIDFtot); //Simpan di TreeMap tfIDF
                        }
                    }
                    // Tulis hasil hitung rata-rata ke file output namafile_tfidf_stat.txt
                    BufferedWriter writeStat = new BufferedWriter(new FileWriter((namaFile + "_tfidf_stat.txt"), true));
                    for (Iterator<Map.Entry<String, Double>> it = entriesSortedByValues(tfIDF).iterator(); it.hasNext();) {
                        Map.Entry<String, Double> entry = it.next();
                        String oneWord = entry.getKey();
                        Double oneValue = entry.getValue();
                        Integer dfreq = docFreq.get(oneWord);
                        //System.out.println("tdidf("+oneWord+")= "+oneValue);
                        writeStat.write(oneWord + " = " + oneValue + ", df = " + dfreq);
                        writeStat.newLine();
                    }
                    writeStat.close();
                }
            } catch (Exception e) {
                System.out.println(e.toString());
            }
            System.out.println("unik: " + docFreq.size());
            System.out.println("Jumlah document:" + arrTweets.size());
            System.out.println("Total term: " + totalTerms);
        }
    }

    public void StemmingFile(String fileIn, String fileOut) {
        try {
            porterStemmer stemmer = new porterStemmer();
            String text = new Scanner(new File(fileIn)).useDelimiter("\\A").next();
            PrintWriter pfo = new PrintWriter(fileOut);
            String[] baris = text.split("\n");
            for (String b : baris) {
                String kata[] = b.split(" ");
                for (int i = 0; i < kata.length; i++) {
                    stemmer.setCurrent(kata[i]);
                    if (stemmer.stem()) {
                        temp = temp + " " + stemmer.getCurrent();
                    } else {
                        temp = temp + " " + kata[i];
                    }
                }
                temp = temp.substring(1, temp.length());
                pfo.println(temp);
                pfo.flush();
                temp = "";
            }
        } catch (IOException e) {
            System.out.println(e.toString());
        }
    }

    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        jScrollPane1 = new javax.swing.JScrollPane();
        hasilconvert = new javax.swing.JTextArea();
        browse = new javax.swing.JToggleButton();
        proses = new javax.swing.JToggleButton();
        jScrollPane2 = new javax.swing.JScrollPane();
        hasilstopword = new javax.swing.JTextArea();
        jScrollPane3 = new javax.swing.JScrollPane();
        hasilstemming = new javax.swing.JTextArea();
        jScrollPane4 = new javax.swing.JScrollPane();
        tfidf = new javax.swing.JTextArea();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        hasilconvert.setColumns(20);
        hasilconvert.setRows(5);
        hasilconvert.setBorder(javax.swing.BorderFactory.createTitledBorder("file pdf"));
        jScrollPane1.setViewportView(hasilconvert);

        browse.setText("browse file");
        browse.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                browseActionPerformed(evt);
            }
        });

        proses.setText("proses file");
        proses.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                prosesActionPerformed(evt);
            }
        });

        hasilstopword.setColumns(20);
        hasilstopword.setRows(5);
        hasilstopword.setBorder(javax.swing.BorderFactory.createTitledBorder("stopword"));
        jScrollPane2.setViewportView(hasilstopword);

        hasilstemming.setColumns(20);
        hasilstemming.setRows(5);
        hasilstemming.setBorder(javax.swing.BorderFactory.createTitledBorder("stemming"));
        jScrollPane3.setViewportView(hasilstemming);

        tfidf.setColumns(20);
        tfidf.setRows(5);
        tfidf.setBorder(javax.swing.BorderFactory.createTitledBorder("TF - IDF"));
        jScrollPane4.setViewportView(tfidf);

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addContainerGap()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(jScrollPane1, javax.swing.GroupLayout.PREFERRED_SIZE, 115, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addGap(18, 18, 18)
                        .addComponent(jScrollPane2, javax.swing.GroupLayout.DEFAULT_SIZE, 280, Short.MAX_VALUE)
                        .addGap(18, 18, 18)
                        .addComponent(jScrollPane3, javax.swing.GroupLayout.PREFERRED_SIZE, 379, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addGap(18, 18, 18)
                        .addComponent(jScrollPane4, javax.swing.GroupLayout.PREFERRED_SIZE, 338, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(browse)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(proses)
                        .addGap(0, 0, Short.MAX_VALUE)))
                .addContainerGap())
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                    .addComponent(browse, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                    .addComponent(proses, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.UNRELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(jScrollPane2)
                    .addComponent(jScrollPane3, javax.swing.GroupLayout.DEFAULT_SIZE, 417, Short.MAX_VALUE)
                    .addComponent(jScrollPane4, javax.swing.GroupLayout.Alignment.TRAILING, javax.swing.GroupLayout.DEFAULT_SIZE, 417, Short.MAX_VALUE)
                    .addComponent(jScrollPane1))
                .addGap(67, 67, 67))
        );

        pack();
    }// </editor-fold>                        

    private void prosesActionPerformed(java.awt.event.ActionEvent evt) {                                       
        try {
            tweet = tweet.trim().replaceAll("\\s+", " ");
            tweet = Normalizer.normalize(tweet, Normalizer.Form.NFD);
            String baru = tweet.toLowerCase();
            String baru2 = baru.replaceAll("[^a-z']", " ");
            String baru3 = baru2.replaceAll("( )+", " ");
            System.out.println("text awal ;" + baru);
            System.out.println("tokenisasi ;" + baru3);
            String baru4 = baru3.replaceAll(" ", "|");
            System.out.println("index ;" + baru4);

            PrintWriter cetak = null;
            cetak = new PrintWriter(new FileWriter("tokenisasi.txt"));
            BufferedWriter bw1 = new BufferedWriter(cetak);
            cetak.println(baru3);
            bw1.close();
            cetak.close();

            Scanner baca = new Scanner(new File("tokenisasi.txt"));
            while (baca.hasNext()) {
                int flag = 1;
                s1 = baca.next();
                s1 = s1.toLowerCase();
                for (int i = 0; i < stopwords.length; i++) {
                    if (s1.equals(stopwords[i])) {
                        flag = 0;
                    }
                }
                if (flag != 0) {
                    hasilstopword.append(s1 + " \n");
                }
            }
            FileWriter writer2 = null;
            writer2 = new FileWriter("stopwords.txt");
            BufferedWriter bw2 = new BufferedWriter(writer2);
            hasilstopword.write(bw2);
            bw2.close();
            hasilstopword.requestFocus();
            writer2.close();
            NewJFrame t = new NewJFrame();
            t.StemmingFile("stopwords.txt", "stemming.txt");
            String printstemming = new Scanner(new File("stemming.txt")).useDelimiter("\\A").next();
            hasilstemming.append(printstemming);
            coba pt = new coba();
            pt.process("stemming.txt", true);
            String printtfidf = new Scanner(new File("stemming_tfidf_stat.txt")).useDelimiter("\\A").next();
            tfidf.append(printtfidf);

            proses.setVisible(false);
        } catch (IOException ex) {
            Logger.getLogger(NewJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
    }                                      

    private void browseActionPerformed(java.awt.event.ActionEvent evt) {                                       
        try {
            JFileChooser chooser = new JFileChooser("C:\\Users\\nickname\\Downloads\\Documents");
            if (chooser.showOpenDialog(this) == JFileChooser.APPROVE_OPTION) {
                path = chooser.getSelectedFile().getAbsolutePath();
                hasilconvert.setText("");
                hasilstopword.setText("");
                hasilstemming.setText("");
                BodyContentHandler handler = new BodyContentHandler(batas);
                Metadata metadata = new Metadata();
                FileInputStream inputstream = new FileInputStream(new File(path));
                ParseContext pcontext = new ParseContext();
                PDFParser pdfparser = new PDFParser();
                pdfparser.parse(inputstream, handler, metadata, pcontext);
                hasilconvert.append(handler.toString());
                tweet = handler.toString();
                proses.setVisible(true);
            }
        } catch (IOException ex) {
            Logger.getLogger(NewJFrame.class.getName()).log(Level.SEVERE, null, ex);
        } catch (SAXException ex) {
            Logger.getLogger(NewJFrame.class.getName()).log(Level.SEVERE, null, ex);
        } catch (TikaException ex) {
            Logger.getLogger(NewJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
    }                                      

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(NewJFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(NewJFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(NewJFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(NewJFrame.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new NewJFrame().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JToggleButton browse;
    private javax.swing.JTextArea hasilconvert;
    private javax.swing.JTextArea hasilstemming;
    private javax.swing.JTextArea hasilstopword;
    private javax.swing.JScrollPane jScrollPane1;
    private javax.swing.JScrollPane jScrollPane2;
    private javax.swing.JScrollPane jScrollPane3;
    private javax.swing.JScrollPane jScrollPane4;
    private javax.swing.JToggleButton proses;
    private javax.swing.JTextArea tfidf;
    // End of variables declaration                   

}
