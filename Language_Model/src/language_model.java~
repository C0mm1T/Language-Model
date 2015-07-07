import java.io.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class language_model {

	private static Map<Object, Object> wordmap;
	private static Map<Object, Object> dictmap;
	private static Map<Object, Object> cv;
	private static Map<Object, Object> li;
	private static String[][] Numbers;
	private static String[][] Numn;
	private static int countdict=0;
	private static BufferedReader Dict=null;
	private static Scanner scanner = new Scanner(System.in);
	private static String folderout = "/home/soninod/Documents/Language_Model/Language_Model/files_out";
	
	public static void main(String[] args) throws IOException {
		File folderin = new File("/home/soninod/Documents/Language_Model/Language_Model/files_in");
		init_c2v();
		init_init_phoneme();
		initReadWord();
		dict();
		initNumbers();
		listFilesForFolder(folderin);
	}
	public static void listFilesForFolder(final File folder) throws FileNotFoundException {
		BufferedReader in = null;
		BufferedWriter out = null;
		String filename=null,list_fl=null;
		String sCurrentLine;
		String word="";
		String text="";
		int up=0,count=0;
		File file;
    	try {
    		for (final File fileEntry : folder.listFiles()) {
    			countdict++;
		        if (fileEntry.isDirectory()) {
		            listFilesForFolder(fileEntry);
		        } else {
		        	list_fl = fileEntry.getName();
		        	filename = "/home/soninod/Documents/Language_Model/Language_Model/files_in/"+fileEntry.getName().toString();
		        	in = new BufferedReader(new FileReader(filename));
		        	file = new File(folderout+"/"+list_fl);
		        	if (file.createNewFile()){
		        		out = new BufferedWriter(new FileWriter(file.getAbsoluteFile()));
		    	      }else{
		    	        System.out.println("File already exists.");
		    	        System.exit(0);
		    	     }
		        	while ((sCurrentLine = in.readLine()) != null) {
		        		sCurrentLine = clean_non_word(sCurrentLine);
						for (String retval1: sCurrentLine.split(" ")){
							text = retval1;
							text = clean_non_word(text);
							text = searchNumbers(text);
							text = searchAbbreviationFromToken(text);
							text = text.toUpperCase();
							if (text.charAt(0) >='A' && text.charAt(0) <='Z') {
								for (int i = 0; i < text.length(); i++) {
									if ((text.charAt(i) >='A' && text.charAt(i) <='Z')) up++;
									else text = text.toLowerCase();
								}
								if (up==text.length()||up!=0) {
									up=0; text = text.toUpperCase();
								}
								String word1="",str1="",word2="";
								for (int i=0; i<text.length(); i++){
									if ((text.charAt(i) >='A' && text.charAt(i) <='Z')) 
										word1=word1+text.charAt(i);
								}
								if (null!=wordmap.get(word1)) {
									for (String str: wordmap.get(word1).toString().split(" ")){
										for (int i=0; i<str.length(); i++){
											if ((str.charAt(i) >='A' && str.charAt(i) <='Z'))
												str1=str1+str.charAt(i);
										}
										word2 = word2+li.get(str1); str1="";
									}
									count=1; word = word+" "+word2;
								}else{
									if (null!=dictmap.get(word1)) {
											word = word +" "+ dictmap.get(word1);
											count=1;
									}else{
										if (countdict%2==0) {
											System.out.println(word1);
											String inputDict="";
											inputDict = scanner.next();
											word = word +" "+ inputDict;
											addDict(word1+" "+inputDict);
											count=1;
										}
									}
								}
							}
							if (1!=count){ word = word +" "+text;} else{ count=0;}
						}
						out.write("<s>"+word.toLowerCase()+" </s>\n");word="";text="";
					}
		        	out.close();
		        }
			}System.out.println("Done!");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	private static void addDict(String string) throws IOException {
		BufferedWriter out = null;
		BufferedReader in = null;
		in = new BufferedReader(new FileReader("dict.txt"));
		String sCurrentLine="";
		String Line="";
		while ((sCurrentLine = in.readLine()) != null) {
			Line=Line+""+sCurrentLine+"\n";
		}
		Line = Line+""+string;
		out = new BufferedWriter(new FileWriter("dict.txt"));
		out.write(Line);
		out.close();
		in.close();
	}
	private static void dict() {
		String[] word = null;
		String phoneme="";
		dictmap = new HashMap<>();
		int i=1;
		try {
			Dict = new BufferedReader(new FileReader("dict.txt"));
			String line = null;
			while ((line = Dict.readLine()) != null) {
				for (String retval: line.split(" ", 2)){
					word = line.split(" ", 2);
					if (i/2!=0) {
						phoneme = retval;
						dictmap.put(word[0],phoneme);
					}
					i++;
			    }
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	private static String searchAbbreviationFromToken(String text) {
		String word = null;
		int j = 1;
		try {
			Dict = new BufferedReader(new FileReader("DictionaryAbbrevation0.txt"));
			String line = null;
			char chars = ' ';
			while ((line = Dict.readLine()) != null) {
				if (j%2!=0) {
					for (int i=0; i<text.length(); i++){
						if (('А'<=text.charAt(i) && text.charAt(i)<='Я') || (chars=='Ү' || chars=='Ө')) {
							word=word+text.charAt(i);
						} else {
							if (line.equals(word)){
								return Dict.readLine();
							}
							word="";
						}
					}
				}
				j++;
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
		return text;
	}

	private static void initNumbers() {
		Numn = new String[2][10];
		Numbers = new String[2][10];
		Numbers[0][0]="тэг";
		Numbers[0][1]="нэг";
		Numbers[0][2]="хоёр";
		Numbers[0][3]="гурав";
		Numbers[0][4]="дөрөв";
		Numbers[0][5]="тав";
		Numbers[0][6]="зургаа";
		Numbers[0][7]="долоо";
		Numbers[0][8]="найм";
		Numbers[0][9]="ес";
		Numbers[1][0]="";
		Numbers[1][1]="арав";
		Numbers[1][2]="хорь";
		Numbers[1][3]="гуч";
		Numbers[1][4]="дөч";
		Numbers[1][5]="тавь";
		Numbers[1][6]="жар";
		Numbers[1][7]="дал";
		Numbers[1][8]="ная";
		Numbers[1][9]="ер";
		Numn[0][0]="";
		Numn[0][1]="нэг";
		Numn[0][2]="хоёр";
		Numn[0][3]="гурван";
		Numn[0][4]="дөрвөн";
		Numn[0][5]="таван";
		Numn[0][6]="зургаан";
		Numn[0][7]="долоон";
		Numn[0][8]="найман";
		Numn[0][9]="есөн";
		Numn[1][0]="";
		Numn[1][1]="арван";
		Numn[1][2]="хорин";
		Numn[1][3]="гучин";
		Numn[1][4]="дөчин";
		Numn[1][5]="тавин";
		Numn[1][6]="жаран";
		Numn[1][7]="далан";
		Numn[1][8]="наян";
		Numn[1][9]="ерэн";
		
	}

	private static String searchNumbers(String text) {
		String ret="";
		boolean isNumber=false;
		int k=0;
		for (int i=0; i<text.length(); i++){
			if ('0'<=text.charAt(i) && text.charAt(i)<='9') {
				isNumber = true;
				k=k*10+text.charAt(i)-'0';
			} else {
				if (isNumber){
					ret=ret+NumberToString(k)+" ";
				}
				isNumber=false; k=0;
				ret+=text.charAt(i);
			}
		}
		if (isNumber){
			ret=ret+NumberToString(k);	
		}
		return ret;
	}

	private static String NumberToString(int k) {
		String ret="";
		String space="";
		if (k==0) return ret="тэг";
		if (k<1000000000) {
			if (k>=1000000)	{ret+=SmallNumber(k/1000000,true)+" сая"; k%=1000000; space=" ";}
			if (k>=1000) {ret+=space+SmallNumber(k/1000,true)+" мянга"; k%=1000; space=" ";}
			if (k!=0) {ret+=space+SmallNumber(k,false);}
		}
		return ret;
	}

	private static String SmallNumber(int k, boolean ok) {
		String ret="";
		String space="";
		if (ok){
			if (k>=100) {ret+=Numn[0][k/100]+" зуун"; k%=100; space=" ";}
			if (k>=10)  {ret+=space+Numn[1][k/10]; space=" "; k%=10;}
			if (k>=2) ret+=space+Numn[0][k]; 
			if (k==1) if(space==" ") ret+=" нэгэн"; else ret+="нэг";
		} else {
			if (k>=100) {ret+=Numn[0][k/100]; k%=100; space=" ";}
			if (space==" ") 
				if (k==0)
					{ret+=" зуу"; return ret;} 
				else ret+=" зуун "; 
			space="";
			if (k%10==0){ ret+=Numbers[1][k/10];} 
			else {
				ret+=Numn[1][k/10]+" "+Numbers[0][k%10]; 
			}
		}
		return ret;
	}

	private static String clean_non_word(String text) {
		String word="";
		int n = text.length();
		char chars = ' ';
		for (int i = 0; i < n; i++) {
			chars = text.charAt(i);
			if ((chars >='а' && chars <='я') || (chars == 'ү' || chars == 'ө') || 
					(chars >='a' && chars <='z') || (chars>='0' && chars<='9') ||
					(chars >='A' && chars <='Z') ||
					(chars >='А' && chars <='Я') || (chars == 'Ү' || chars == 'Ө')) {
				word = word + chars;
			}
			if (chars == '-' || chars == ' ') {
				word = word + ' '; 
			}
		}
		return word;
	}

	private static void init_init_phoneme() {
		li = new HashMap<Object, Object>();
		li.put("AA","О");
		li.put("AE","А");
		li.put("AH","АЙ");
		li.put("AO","ОЙ");
		li.put("AW","ОВ");
		li.put("AY","АЙ");
		li.put("B","Б");
		li.put("CH","Ч");
		li.put("D","Д");
		li.put("DH","Д");
		li.put("EH","И");
		li.put("ER","Р");
		li.put("EY","АЙ");
		li.put("F","П");
		li.put("G","Г");
		li.put("HH","Х");
		li.put("IH","И");
		li.put("IY","И");
		li.put("JH","Ж");
		li.put("K","К");
		li.put("L","Л");
		li.put("M","М");
		li.put("N","Н");
		li.put("NG","НГ");
		li.put("OW","О");
		li.put("OY","ОЙ");
		li.put("P","П");
		li.put("R","Р");
		li.put("S","С");
		li.put("SH","Ш");
		li.put("T","Т");
		li.put("TH","Т");
		li.put("UH","Х");
		li.put("UW","Ү");
		li.put("V","В");
		li.put("W","В");
		li.put("Y","И");
		li.put("Z","З");
		li.put("ZH","З");
	}

	private static void init_c2v() {
		cv = new HashMap<Object, Object>();
		cv.put("A","А");
		cv.put("S","С");
		cv.put("D","Д");
		cv.put("F","П");
		cv.put("G","Г");
		cv.put("H","Х");
		cv.put("J","Ж");
		cv.put("K","К");
		cv.put("L","Л");
		cv.put("Q","К");
		cv.put("W","В");
		cv.put("E","И");
		cv.put("R","Р");
		cv.put("T","Т");
		cv.put("Y","И");
		cv.put("U","У");
		cv.put("I","И");
		cv.put("O","О");
		cv.put("P","П");
		cv.put("Z","З");
		cv.put("X","Х");
		cv.put("C","С");
		cv.put("V","В");
		cv.put("B","Б");
		cv.put("N","Н");
		cv.put("M","М");
	}

	private static void initReadWord() throws IOException {
		BufferedReader in = null;
		try {
 
			String sCurrentLine;
			String[] word=null;
			wordmap = new HashMap<>();
			int i=1;
			String phoneme;
			in = new BufferedReader(new FileReader("cmudict"));
 
			while ((sCurrentLine = in.readLine()) != null) {
				for (String retval: sCurrentLine.split(" ", 2)){
					word = sCurrentLine.split(" ", 2);
					if (i/2!=0) {
						phoneme = retval.substring(2, retval.length());
						
						wordmap.put(word[0],check_phoneme(phoneme));
					}
					i++;
			    }
				i=1;
			}
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				if (in != null ){in.close();}
			} catch (IOException ex) {
				ex.printStackTrace();
			}
		}
	}

	@SuppressWarnings("unused")
	private static String search(String retval) {
		String word="";
		int n = retval.length();
		char chars = ' ';
		for (int i = 0; i < n; i++) {
			chars = retval.charAt(i);
			if (chars>47 && chars<48 || chars>57)
				if (chars!='.') 
					word =word+chars;
		}
		return word;
	}
	
	public static String check_phoneme(String string1){
		String word="";
		int n = string1.length();
		char chars = ' ';
		for (int i = 0; i < n; i++) {
			chars = string1.charAt(i);
				if (chars != '.' && chars != '-') 
					word =word+chars;
		}
		return word;
	} 
	
}
