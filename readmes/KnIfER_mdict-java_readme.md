# MDict Library in pure java ！

![image](https://github.com/KnIfER/mdict-java/raw/master/screenshots/PDPC.png)  

It supports:  
&nbsp;&nbsp;&nbsp;I.Lzo compressed contents. (via lzo-core)  
&nbsp;&nbsp;II.Ripemd128 key-info decryption.  
&nbsp;III.Builders to make Mdx add Mdd.  

and is able to do:  
&nbsp;&nbsp;&nbsp;I.Basic query.  
&nbsp;&nbsp;II.Conjuction search.  
&nbsp;III.Fast wildcard match among entries.  
&nbsp;IV.Fast Fulltext retrieval. (also with wild cards)  

# Android App
https://github.com/KnIfER/PlainDictionaryAPP

# Usage:
### 1.Basic query:
```
String key = "happy";
mdict md = new mdict(path);
int search_result = md.lookUp(key, true);//true means to match strictly  
if(search_result>=0){
  String html_contents = md.getRecordAt(search_result);
  String entry_name = md.getEntryAt(search_result);
}
```
### 2.Search in a bunch of dicts:
```
key = "happy";
ArrayList<mdict> mdxs = new ArrayList<>();
...
RBTree_additive combining_search_tree = new RBTree_additive();
for(int i=0;i<mdxs.size();i++)
{
  mdxs.get(i).size_confined_lookUp(key,combining_search_tree,i,30);
}  	
combining_search_tree.inOrder();//print results stored in the RBTree

/*printed results looks like 【happy____@111@0@222@1@16906@1】...【other results】...
how to handle:
String html_contents0 = mdxs.get(0).getRecordAt(111);
...
...  
...
*/
```


# details
* This project was initially converted from xiaoqiangWang's [python analyzer](https://bitbucket.org/xwang/mdict-analysis). 
* Use [red-black tree](http://www.cnblogs.com/skywang12345/p/3245399.html) and binary-list-searching(mainly) to implement dictionary funcitons.  
* Feng Dihai(@[fengdh](https://github.com/fengdh/mdict-js))'s mdict-js is of help too, I've just switched to use the same short but elegant binary-list-searching method——reduce().Somehow, this function always returns the first occurence of the entry >= keyword, in a pre-sorted list that contain entries. maybe some mathematician could tell me why, but I've tested over 100000 times without any expectation.
* Maybe I should oneday replace red-black tree and the recursive reduce method with `Arrays.binarySearch`, but I am lazy... 
```
/*via mdict-js
 *note at first time we feed in 0 as start and array.length as end. it must not be array.length-1. 
*/
public static int reduce(int phrase, int[] array,int start,int end) {
	int len = end-start;
	if (len > 1) {
	  len = len >> 1;
	  return phrase > array[start + len - 1]
				? reduce(phrase,array,start+len,end)
				: reduce(phrase,array,start,start+len);
	} else {
	  return start;
	}
}
```
	
	
MDX File Format
===============
<img src="https://github.com/KnIfER/mdict-java/blob/master/screenshots/mdx.svg">


MDD File Format
===============
<img src="https://rawgit.com/csarron/mdict-analysis/master/MDD.svg">

Source Code License: 
Apache2.0 for the core part, specifically anything under the package of com.knziha.plod.dictionary.*; GPL3.0 for everything else including the mdictBuilder, UI part, and the android application. 
As for the License of mdx file format itself, well, you know, mdict is an open dictionary platform.
