# Auto-Correct-
import java.util.ArrayList;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
class TrieNode{
	char data;
	boolean isTerminating;
	TrieNode children[];
	int childCount;

	public TrieNode(char data) {
		this.data = data;
		isTerminating = false;
		children = new TrieNode[26];
		childCount = 0;
	}
}

public class Trie {
	private TrieNode root;
	
	public Trie() {
		root = new TrieNode('\0');
	}

	private void add(TrieNode root, String word){
		if(word.length() == 0){
			root.isTerminating = true;
			return;
		}		
		int childIndex = word.charAt(0) - 'a';
		TrieNode child = root.children[childIndex];
		if(child == null){
			child = new TrieNode(word.charAt(0));
			root.children[childIndex] = child;
			root.childCount++;
		}
		add(child, word.substring(1));
	}

	public void add(String word){
		add(root, word);
	}
	
    public TrieNode findword(TrieNode root,String word){
        if(word.length() == 0){
            return root;
        }
        int childIndex = word.charAt(0)-'a';
        TrieNode child = root.children[childIndex];
        if(child == null){
            return null;
        }
        return findword(child,word.substring(1));
    }
    
    public void allwords(TrieNode root,String word,String output){
        if(root.childCount==0){
            if(output.length()>0){
                System.out.println(word+output);
            }
            return;
        }
        if(root.isTerminating){
            System.out.println(word+output);
        }
        for(int i=0;i<root.children.length;i++){
            if(root.children[i]!=null){
                String ans = output+root.children[i].data;
                allwords(root.children[i],word,ans);
            }
        }
    }
	
	public void autoComplete(ArrayList<String> input, String word) {
        for(int i=0;i<input.size();i++){
            add(input.get(i));
        }
        TrieNode a = findword(root,word);
        if(a ==null || a.childCount ==0){
            return;
        }
        
        String output = "";
        allwords(a,word,output);
	}
}
public class Runner {

	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    
	
	public static void main(String[] args) throws IOException{
		Trie t = new Trie();
		int n = Integer.parseInt(br.readLine().trim());
        ArrayList<String> input = new ArrayList<String>();
        String[] words = br.readLine().split("\\s");
		for(int i = 0; i < n; i++) {
			
			input.add(words[i]);
		}
		String pattern = br.readLine();
		t.autoComplete(input, pattern);
	}
}
