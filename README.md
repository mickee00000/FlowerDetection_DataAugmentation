# FlowerDetection_DataAugmentation

<html>
<head>
<title>26_Self_Data_Augmentation.ipynb</title>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<style type="text/css">
.s0 { color: #69676c; font-style: italic;}
.s1 { color: #fc618d;}
.s2 { color: #f7f1ff;}
.s3 { color: #8b888f;}
.s4 { color: #fce566;}
.s5 { color: #948ae3;}
.ln { color: #525053; font-weight: normal; font-style: normal; }
</style>
</head>
<body bgcolor="#222222">
<table CELLSPACING=0 CELLPADDING=5 COLS=1 WIDTH="100%" BGCOLOR="#606060" >
<tr><td><center>
<font face="Arial, Helvetica" color="#000000">
26_Self_Data_Augmentation.ipynb</font>
</center></td></tr></table>
<pre><a name="l1"><span class="ln">1    </span></a><span class="s0">#%% 
<a name="l2"><span class="ln">2    </span></a></span><span class="s1">import </span><span class="s2">matplotlib</span><span class="s3">.</span><span class="s2">pyplot </span><span class="s1">as </span><span class="s2">plt</span>
<a name="l3"><span class="ln">3    </span></a><span class="s1">import </span><span class="s2">pandas </span><span class="s1">as </span><span class="s2">pd</span>
<a name="l4"><span class="ln">4    </span></a><span class="s1">import </span><span class="s2">numpy </span><span class="s1">as </span><span class="s2">np</span>
<a name="l5"><span class="ln">5    </span></a><span class="s1">import </span><span class="s2">tensorflow </span><span class="s1">as </span><span class="s2">tf</span>
<a name="l6"><span class="ln">6    </span></a><span class="s1">from </span><span class="s2">tensorflow </span><span class="s1">import </span><span class="s2">keras</span>
<a name="l7"><span class="ln">7    </span></a><span class="s0">#%% 
<a name="l8"><span class="ln">8    </span></a></span><span class="s1">import </span><span class="s2">PIL</span>
<a name="l9"><span class="ln">9    </span></a><span class="s1">import </span><span class="s2">os</span>
<a name="l10"><span class="ln">10   </span></a><span class="s1">import </span><span class="s2">cv2</span>
<a name="l11"><span class="ln">11   </span></a><span class="s1">import </span><span class="s2">pathlib</span>
<a name="l12"><span class="ln">12   </span></a><span class="s0">#%% 
<a name="l13"><span class="ln">13   </span></a></span><span class="s2">flower_data </span><span class="s1">= </span><span class="s2">tf</span><span class="s3">.</span><span class="s2">keras</span><span class="s3">.</span><span class="s2">utils</span><span class="s3">.</span><span class="s2">get_file</span><span class="s3">(</span><span class="s4">'flower_photos'</span><span class="s3">, </span><span class="s2">origin </span><span class="s1">= </span><span class="s4">'https://storage.googleapis.com/download.tensorflow.org/example_images/flower_photos.tgz'</span><span class="s3">, </span><span class="s2">cache_dir</span><span class="s1">=</span><span class="s4">'.'</span><span class="s3">, </span><span class="s2">untar</span><span class="s1">=True</span><span class="s3">)</span>
<a name="l14"><span class="ln">14   </span></a><span class="s0">#%% 
<a name="l15"><span class="ln">15   </span></a></span><span class="s2">flower_data</span>
<a name="l16"><span class="ln">16   </span></a><span class="s0">#%% 
<a name="l17"><span class="ln">17   </span></a></span><span class="s2">flower_data </span><span class="s1">= </span><span class="s2">pathlib</span><span class="s3">.</span><span class="s2">Path</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">)</span>
<a name="l18"><span class="ln">18   </span></a><span class="s0">#%% 
<a name="l19"><span class="ln">19   </span></a></span><span class="s2">flower_data</span>
<a name="l20"><span class="ln">20   </span></a><span class="s0">#%% 
<a name="l21"><span class="ln">21   </span></a></span><span class="s2">image_count </span><span class="s1">= </span><span class="s2">len</span><span class="s3">(</span><span class="s2">list</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">.</span><span class="s2">glob</span><span class="s3">(</span><span class="s4">'*/*.jpg'</span><span class="s3">)))</span>
<a name="l22"><span class="ln">22   </span></a><span class="s0">#%% 
<a name="l23"><span class="ln">23   </span></a></span><span class="s2">image_count</span>
<a name="l24"><span class="ln">24   </span></a><span class="s0">#%% 
<a name="l25"><span class="ln">25   </span></a></span><span class="s2">flower_values </span><span class="s1">= </span><span class="s3">{</span>
<a name="l26"><span class="ln">26   </span></a>    <span class="s4">'daisy' </span><span class="s1">: </span><span class="s2">list</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">.</span><span class="s2">glob</span><span class="s3">(</span><span class="s4">'daisy/*'</span><span class="s3">)),</span>
<a name="l27"><span class="ln">27   </span></a>    <span class="s4">'dandelion' </span><span class="s1">: </span><span class="s2">list</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">.</span><span class="s2">glob</span><span class="s3">(</span><span class="s4">'dandelion/*'</span><span class="s3">)),</span>
<a name="l28"><span class="ln">28   </span></a>    <span class="s4">'roses' </span><span class="s1">: </span><span class="s2">list</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">.</span><span class="s2">glob</span><span class="s3">(</span><span class="s4">'roses/*'</span><span class="s3">)),</span>
<a name="l29"><span class="ln">29   </span></a>    <span class="s4">'sunflowers' </span><span class="s1">: </span><span class="s2">list</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">.</span><span class="s2">glob</span><span class="s3">(</span><span class="s4">'sunflowers/*'</span><span class="s3">)),</span>
<a name="l30"><span class="ln">30   </span></a>    <span class="s4">'tulips' </span><span class="s1">: </span><span class="s2">list</span><span class="s3">(</span><span class="s2">flower_data</span><span class="s3">.</span><span class="s2">glob</span><span class="s3">(</span><span class="s4">'tulips/*'</span><span class="s3">)),</span>
<a name="l31"><span class="ln">31   </span></a><span class="s3">}</span>
<a name="l32"><span class="ln">32   </span></a>
<a name="l33"><span class="ln">33   </span></a><span class="s2">flower_label </span><span class="s1">= </span><span class="s3">{</span>
<a name="l34"><span class="ln">34   </span></a>    <span class="s4">'daisy' </span><span class="s1">: </span><span class="s5">0</span><span class="s3">,</span>
<a name="l35"><span class="ln">35   </span></a>    <span class="s4">'dandelion' </span><span class="s1">: </span><span class="s5">1</span><span class="s3">,</span>
<a name="l36"><span class="ln">36   </span></a>    <span class="s4">'roses' </span><span class="s1">: </span><span class="s5">2</span><span class="s3">,</span>
<a name="l37"><span class="ln">37   </span></a>    <span class="s4">'sunflowers' </span><span class="s1">: </span><span class="s5">3</span><span class="s3">,</span>
<a name="l38"><span class="ln">38   </span></a>    <span class="s4">'tulips' </span><span class="s1">: </span><span class="s5">4</span>
<a name="l39"><span class="ln">39   </span></a><span class="s3">}</span>
<a name="l40"><span class="ln">40   </span></a>
<a name="l41"><span class="ln">41   </span></a><span class="s0">#%% 
<a name="l42"><span class="ln">42   </span></a></span><span class="s2">PIL</span><span class="s3">.</span><span class="s2">Image</span><span class="s3">.</span><span class="s2">open</span><span class="s3">(</span><span class="s2">str</span><span class="s3">(</span><span class="s2">flower_values</span><span class="s3">[</span><span class="s4">'roses'</span><span class="s3">][</span><span class="s5">0</span><span class="s3">]))</span>
<a name="l43"><span class="ln">43   </span></a><span class="s0">#%% 
<a name="l44"><span class="ln">44   </span></a></span><span class="s2">plt</span><span class="s3">.</span><span class="s2">matshow</span><span class="s3">(</span><span class="s2">cv2</span><span class="s3">.</span><span class="s2">imread</span><span class="s3">(</span><span class="s2">str</span><span class="s3">(</span><span class="s2">flower_values</span><span class="s3">[</span><span class="s4">'roses'</span><span class="s3">][</span><span class="s5">0</span><span class="s3">])))</span>
<a name="l45"><span class="ln">45   </span></a><span class="s0">#%% 
<a name="l46"><span class="ln">46   </span></a></span><span class="s2">X </span><span class="s1">= </span><span class="s3">[]</span>
<a name="l47"><span class="ln">47   </span></a><span class="s2">y </span><span class="s1">= </span><span class="s3">[]</span>
<a name="l48"><span class="ln">48   </span></a><span class="s0">#%% 
<a name="l49"><span class="ln">49   </span></a></span><span class="s1">for </span><span class="s2">flower_name </span><span class="s3">, </span><span class="s2">flower_list </span><span class="s1">in </span><span class="s2">flower_values</span><span class="s3">.</span><span class="s2">items</span><span class="s3">()</span><span class="s1">:</span>
<a name="l50"><span class="ln">50   </span></a>    <span class="s1">for </span><span class="s2">flower </span><span class="s1">in </span><span class="s2">flower_list</span><span class="s1">:</span>
<a name="l51"><span class="ln">51   </span></a>        <span class="s2">img </span><span class="s1">= </span><span class="s2">cv2</span><span class="s3">.</span><span class="s2">imread</span><span class="s3">(</span><span class="s2">str</span><span class="s3">(</span><span class="s2">flower</span><span class="s3">))</span>
<a name="l52"><span class="ln">52   </span></a>        <span class="s2">img </span><span class="s1">= </span><span class="s2">cv2</span><span class="s3">.</span><span class="s2">resize</span><span class="s3">(</span><span class="s2">img</span><span class="s3">, (</span><span class="s5">180</span><span class="s3">,</span><span class="s5">180</span><span class="s3">))</span>
<a name="l53"><span class="ln">53   </span></a>        <span class="s2">X</span><span class="s3">.</span><span class="s2">append</span><span class="s3">(</span><span class="s2">img</span><span class="s3">)</span>
<a name="l54"><span class="ln">54   </span></a>        <span class="s2">y</span><span class="s3">.</span><span class="s2">append</span><span class="s3">(</span><span class="s2">flower_label</span><span class="s3">[</span><span class="s2">flower_name</span><span class="s3">])</span>
<a name="l55"><span class="ln">55   </span></a><span class="s0">#%% 
<a name="l56"><span class="ln">56   </span></a></span><span class="s2">X </span><span class="s1">= </span><span class="s2">np</span><span class="s3">.</span><span class="s2">array</span><span class="s3">(</span><span class="s2">X</span><span class="s3">)</span>
<a name="l57"><span class="ln">57   </span></a><span class="s2">y </span><span class="s1">= </span><span class="s2">np</span><span class="s3">.</span><span class="s2">array</span><span class="s3">(</span><span class="s2">y</span><span class="s3">)</span>
<a name="l58"><span class="ln">58   </span></a><span class="s0">#%% md 
<a name="l59"><span class="ln">59   </span></a></span>
<a name="l60"><span class="ln">60   </span></a><span class="s0">#%% 
<a name="l61"><span class="ln">61   </span></a></span><span class="s2">X </span><span class="s1">= </span><span class="s2">X </span><span class="s1">/ </span><span class="s5">255</span>
<a name="l62"><span class="ln">62   </span></a><span class="s0">#%% 
<a name="l63"><span class="ln">63   </span></a></span><span class="s2">y </span><span class="s1">= </span><span class="s2">keras</span><span class="s3">.</span><span class="s2">utils</span><span class="s3">.</span><span class="s2">to_categorical</span><span class="s3">(</span><span class="s2">y</span><span class="s3">)</span>
<a name="l64"><span class="ln">64   </span></a><span class="s0">#%% 
<a name="l65"><span class="ln">65   </span></a></span><span class="s1">from </span><span class="s2">sklearn</span><span class="s3">.</span><span class="s2">model_selection </span><span class="s1">import </span><span class="s2">train_test_split</span>
<a name="l66"><span class="ln">66   </span></a>
<a name="l67"><span class="ln">67   </span></a><span class="s2">X_train</span><span class="s3">, </span><span class="s2">X_test</span><span class="s3">, </span><span class="s2">y_train</span><span class="s3">, </span><span class="s2">y_test </span><span class="s1">= </span><span class="s2">train_test_split</span><span class="s3">(</span><span class="s2">X</span><span class="s3">, </span><span class="s2">y</span><span class="s3">)</span>
<a name="l68"><span class="ln">68   </span></a><span class="s0">#%% 
<a name="l69"><span class="ln">69   </span></a></span><span class="s2">len</span><span class="s3">(</span><span class="s2">X_train</span><span class="s3">), </span><span class="s2">len</span><span class="s3">(</span><span class="s2">X_test</span><span class="s3">), </span><span class="s2">len</span><span class="s3">(</span><span class="s2">y_train</span><span class="s3">), </span><span class="s2">len</span><span class="s3">(</span><span class="s2">y_test</span><span class="s3">)</span>
<a name="l70"><span class="ln">70   </span></a><span class="s0">#%% 
<a name="l71"><span class="ln">71   </span></a></span><span class="s2">simple_model </span><span class="s1">= </span><span class="s2">keras</span><span class="s3">.</span><span class="s2">Sequential</span><span class="s3">([</span>
<a name="l72"><span class="ln">72   </span></a>
<a name="l73"><span class="ln">73   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Conv2D</span><span class="s3">(</span><span class="s5">16</span><span class="s3">,</span><span class="s5">3</span><span class="s3">,</span><span class="s2">padding</span><span class="s1">=</span><span class="s4">'same'</span><span class="s3">,</span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'relu'</span><span class="s3">),</span>
<a name="l74"><span class="ln">74   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">MaxPooling2D</span><span class="s3">(),</span>
<a name="l75"><span class="ln">75   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Conv2D</span><span class="s3">(</span><span class="s5">32</span><span class="s3">,</span><span class="s5">3</span><span class="s3">,</span><span class="s2">padding</span><span class="s1">=</span><span class="s4">'same'</span><span class="s3">,</span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'relu'</span><span class="s3">),</span>
<a name="l76"><span class="ln">76   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">MaxPooling2D</span><span class="s3">(),</span>
<a name="l77"><span class="ln">77   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Conv2D</span><span class="s3">(</span><span class="s5">63</span><span class="s3">,</span><span class="s5">3</span><span class="s3">,</span><span class="s2">padding</span><span class="s1">=</span><span class="s4">'same'</span><span class="s3">,</span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'relu'</span><span class="s3">),</span>
<a name="l78"><span class="ln">78   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">MaxPooling2D</span><span class="s3">(),</span>
<a name="l79"><span class="ln">79   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Flatten</span><span class="s3">(),</span>
<a name="l80"><span class="ln">80   </span></a>
<a name="l81"><span class="ln">81   </span></a>
<a name="l82"><span class="ln">82   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Dense</span><span class="s3">(</span><span class="s5">1024</span><span class="s3">, </span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'relu'</span><span class="s3">),</span>
<a name="l83"><span class="ln">83   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Dense</span><span class="s3">(</span><span class="s5">256</span><span class="s3">, </span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'relu'</span><span class="s3">),</span>
<a name="l84"><span class="ln">84   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Dense</span><span class="s3">(</span><span class="s5">64</span><span class="s3">, </span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'relu'</span><span class="s3">),</span>
<a name="l85"><span class="ln">85   </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">Dense</span><span class="s3">(</span><span class="s5">5</span><span class="s3">, </span><span class="s2">activation </span><span class="s1">= </span><span class="s4">'softmax'</span><span class="s3">)</span>
<a name="l86"><span class="ln">86   </span></a><span class="s3">])</span>
<a name="l87"><span class="ln">87   </span></a><span class="s0">#%% 
<a name="l88"><span class="ln">88   </span></a></span><span class="s2">simple_model</span><span class="s3">.</span><span class="s2">compile</span><span class="s3">(</span>
<a name="l89"><span class="ln">89   </span></a>    <span class="s2">optimizer </span><span class="s1">= </span><span class="s4">'adam'</span><span class="s3">,</span>
<a name="l90"><span class="ln">90   </span></a>    <span class="s2">metrics </span><span class="s1">= </span><span class="s3">[</span><span class="s4">'accuracy'</span><span class="s3">],</span>
<a name="l91"><span class="ln">91   </span></a>    <span class="s2">loss </span><span class="s1">= </span><span class="s4">'categorical_crossentropy'</span>
<a name="l92"><span class="ln">92   </span></a><span class="s3">)</span>
<a name="l93"><span class="ln">93   </span></a><span class="s0">#%% 
<a name="l94"><span class="ln">94   </span></a></span><span class="s2">simple_model</span><span class="s3">.</span><span class="s2">fit</span><span class="s3">(</span><span class="s2">X_train</span><span class="s3">, </span><span class="s2">y_train</span><span class="s3">, </span><span class="s2">epochs </span><span class="s1">= </span><span class="s5">30</span><span class="s3">)</span>
<a name="l95"><span class="ln">95   </span></a><span class="s0">#%% 
<a name="l96"><span class="ln">96   </span></a></span><span class="s2">simple_model</span><span class="s3">.</span><span class="s2">evaluate</span><span class="s3">(</span><span class="s2">X_test</span><span class="s3">, </span><span class="s2">y_test</span><span class="s3">)</span>
<a name="l97"><span class="ln">97   </span></a><span class="s0">#%% 
<a name="l98"><span class="ln">98   </span></a></span><span class="s2">y_hat </span><span class="s1">= </span><span class="s2">simple_model</span><span class="s3">.</span><span class="s2">predict</span><span class="s3">(</span><span class="s2">X_test</span><span class="s3">)</span>
<a name="l99"><span class="ln">99   </span></a><span class="s0">#%% 
<a name="l100"><span class="ln">100  </span></a></span><span class="s2">labeled_y_hat </span><span class="s1">= </span><span class="s3">[</span><span class="s2">np</span><span class="s3">.</span><span class="s2">argmax</span><span class="s3">(</span><span class="s2">i</span><span class="s3">) </span><span class="s1">for </span><span class="s2">i </span><span class="s1">in </span><span class="s2">y_hat</span><span class="s3">]</span>
<a name="l101"><span class="ln">101  </span></a><span class="s0">#%% 
<a name="l102"><span class="ln">102  </span></a></span><span class="s2">labeled_y_hat</span>
<a name="l103"><span class="ln">103  </span></a><span class="s0">#%% 
<a name="l104"><span class="ln">104  </span></a></span><span class="s2">augmented_model </span><span class="s1">= </span><span class="s2">keras</span><span class="s3">.</span><span class="s2">Sequential</span><span class="s3">([</span>
<a name="l105"><span class="ln">105  </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">experimental</span><span class="s3">.</span><span class="s2">preprocessing</span><span class="s3">.</span><span class="s2">RandomFlip</span><span class="s3">(</span><span class="s4">&quot;horizontal&quot;</span><span class="s3">, </span><span class="s2">input_shape</span><span class="s1">=</span><span class="s3">(</span><span class="s5">180</span><span class="s3">, </span><span class="s5">180</span><span class="s3">,</span><span class="s5">3</span><span class="s3">)),</span>
<a name="l106"><span class="ln">106  </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">experimental</span><span class="s3">.</span><span class="s2">preprocessing</span><span class="s3">.</span><span class="s2">RandomRotation</span><span class="s3">(</span><span class="s5">0.1</span><span class="s3">),</span>
<a name="l107"><span class="ln">107  </span></a>    <span class="s2">keras</span><span class="s3">.</span><span class="s2">layers</span><span class="s3">.</span><span class="s2">experimental</span><span class="s3">.</span><span class="s2">preprocessing</span><span class="s3">.</span><span class="s2">RandomZoom</span><span class="s3">(</span><span class="s5">0.1</span><span class="s3">),</span>
<a name="l108"><span class="ln">108  </span></a>    <span class="s2">simple_model</span>
<a name="l109"><span class="ln">109  </span></a><span class="s3">])</span>
<a name="l110"><span class="ln">110  </span></a><span class="s0">#%% 
<a name="l111"><span class="ln">111  </span></a></span><span class="s2">augmented_model</span><span class="s3">.</span><span class="s2">compile</span><span class="s3">(</span>
<a name="l112"><span class="ln">112  </span></a>    <span class="s2">optimizer </span><span class="s1">= </span><span class="s4">'adam'</span><span class="s3">,</span>
<a name="l113"><span class="ln">113  </span></a>    <span class="s2">metrics </span><span class="s1">= </span><span class="s3">[</span><span class="s4">'accuracy'</span><span class="s3">],</span>
<a name="l114"><span class="ln">114  </span></a>    <span class="s2">loss </span><span class="s1">= </span><span class="s4">'categorical_crossentropy'</span>
<a name="l115"><span class="ln">115  </span></a><span class="s3">)</span>
<a name="l116"><span class="ln">116  </span></a><span class="s0">#%% 
<a name="l117"><span class="ln">117  </span></a></span><span class="s2">augmented_model</span><span class="s3">.</span><span class="s2">fit</span><span class="s3">(</span><span class="s2">X_train</span><span class="s3">, </span><span class="s2">y_train</span><span class="s3">, </span><span class="s2">epochs </span><span class="s1">= </span><span class="s5">30</span><span class="s3">)</span>
<a name="l118"><span class="ln">118  </span></a><span class="s0">#%% 
<a name="l119"><span class="ln">119  </span></a></span><span class="s2">augmented_model</span><span class="s3">.</span><span class="s2">evaluate</span><span class="s3">(</span><span class="s2">X_test</span><span class="s3">, </span><span class="s2">y_test</span><span class="s3">)</span>
<a name="l120"><span class="ln">120  </span></a><span class="s0">#%% 
<a name="l121"><span class="ln">121  </span></a></span><span class="s2">y_hat </span><span class="s1">= </span><span class="s2">augmented_model</span><span class="s3">.</span><span class="s2">predict</span><span class="s3">(</span><span class="s2">X_test</span><span class="s3">)</span>
<a name="l122"><span class="ln">122  </span></a><span class="s0">#%% 
<a name="l123"><span class="ln">123  </span></a></span><span class="s2">labeled_y_hat </span><span class="s1">= </span><span class="s3">[</span><span class="s2">np</span><span class="s3">.</span><span class="s2">argmax</span><span class="s3">(</span><span class="s2">i</span><span class="s3">) </span><span class="s1">for </span><span class="s2">i </span><span class="s1">in </span><span class="s2">y_hat</span><span class="s3">]</span></pre>
</body>
</html>
