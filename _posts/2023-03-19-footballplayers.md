---
keywords: fastai
title: Football Players SQL Table
toc: true
author: Kalani
comments: true
nb_path: _notebooks/2023-03-19-footballplayers.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2023-03-19-footballplayers.ipynb
-->

<div class="container" id="notebook-container">
        
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">flask</span> <span class="kn">import</span> <span class="n">Flask</span>
<span class="kn">from</span> <span class="nn">flask_sqlalchemy</span> <span class="kn">import</span> <span class="n">SQLAlchemy</span>

<span class="n">app</span> <span class="o">=</span> <span class="n">Flask</span><span class="p">(</span><span class="vm">__name__</span><span class="p">)</span>
<span class="n">database</span> <span class="o">=</span> <span class="s1">&#39;sqlite:///sqlite.db&#39;</span>
<span class="n">app</span><span class="o">.</span><span class="n">config</span><span class="p">[</span><span class="s1">&#39;SQLALCHEMY_TRACK_MODIFICATIONS&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="kc">False</span>
<span class="n">app</span><span class="o">.</span><span class="n">config</span><span class="p">[</span><span class="s1">&#39;SQLALCHEMY_DATABASE_URI&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="n">database</span>
<span class="n">app</span><span class="o">.</span><span class="n">config</span><span class="p">[</span><span class="s1">&#39;SECRET_KEY&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="s1">&#39;SECRET_KEY&#39;</span>
<span class="n">db</span> <span class="o">=</span> <span class="n">SQLAlchemy</span><span class="p">()</span>

<span class="n">db</span><span class="o">.</span><span class="n">init_app</span><span class="p">(</span><span class="n">app</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">datetime</span>
<span class="kn">from</span> <span class="nn">datetime</span> <span class="kn">import</span> <span class="n">datetime</span>
<span class="kn">import</span> <span class="nn">json</span>

<span class="kn">from</span> <span class="nn">sqlalchemy.exc</span> <span class="kn">import</span> <span class="n">IntegrityError</span>
<span class="kn">from</span> <span class="nn">werkzeug.security</span> <span class="kn">import</span> <span class="n">generate_password_hash</span><span class="p">,</span> <span class="n">check_password_hash</span>

<span class="k">class</span> <span class="nc">Player</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Model</span><span class="p">):</span>
    <span class="n">__tablename__</span> <span class="o">=</span> <span class="s1">&#39;football&#39;</span>

    <span class="nb">id</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Integer</span><span class="p">,</span> <span class="n">primary_key</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
    <span class="n">_name</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">String</span><span class="p">(</span><span class="mi">255</span><span class="p">),</span> <span class="n">unique</span><span class="o">=</span><span class="kc">False</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
    <span class="n">_number</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Integer</span><span class="p">,</span> <span class="n">unique</span><span class="o">=</span><span class="kc">False</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
    <span class="n">_wins</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Integer</span><span class="p">,</span> <span class="n">unique</span><span class="o">=</span><span class="kc">False</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
    <span class="n">_losses</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">Column</span><span class="p">(</span><span class="n">db</span><span class="o">.</span><span class="n">Integer</span><span class="p">,</span> <span class="n">unique</span><span class="o">=</span><span class="kc">False</span><span class="p">,</span> <span class="n">nullable</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>

    <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">name</span><span class="p">,</span> <span class="n">number</span><span class="p">,</span> <span class="n">wins</span><span class="p">,</span> <span class="n">losses</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_name</span> <span class="o">=</span> <span class="n">name</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_number</span> <span class="o">=</span> <span class="n">number</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_wins</span> <span class="o">=</span> <span class="n">wins</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_losses</span> <span class="o">=</span> <span class="n">losses</span>

   
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">name</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_name</span>
    
    <span class="nd">@name</span><span class="o">.</span><span class="n">setter</span> 
    <span class="k">def</span> <span class="nf">name</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">name</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_name</span> <span class="o">=</span> <span class="n">name</span>
    
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">number</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_number</span>
    
    <span class="nd">@number</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">number</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">number</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_number</span> <span class="o">=</span> <span class="n">number</span>
    
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">wins</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_wins</span>

    <span class="nd">@wins</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">wins</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">wins</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_wins</span> <span class="o">=</span> <span class="n">wins</span> 
    
    <span class="nd">@property</span>
    <span class="k">def</span> <span class="nf">losses</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">_losses</span>

    <span class="nd">@losses</span><span class="o">.</span><span class="n">setter</span>
    <span class="k">def</span> <span class="nf">losses</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">losses</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">_losses</span> <span class="o">=</span> <span class="n">losses</span>


    <span class="k">def</span> <span class="fm">__str__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="n">json</span><span class="o">.</span><span class="n">dumps</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">read</span><span class="p">())</span>

    <span class="k">def</span> <span class="nf">create</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">try</span><span class="p">:</span>
            
            <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="bp">self</span><span class="p">)</span> 
            <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span> 
            <span class="k">return</span> <span class="bp">self</span>
        <span class="k">except</span> <span class="n">IntegrityError</span><span class="p">:</span>
            <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">remove</span><span class="p">()</span>
            <span class="k">return</span> <span class="kc">None</span>

    <span class="k">def</span> <span class="nf">read</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="p">{</span>
            <span class="s2">&quot;id&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">id</span><span class="p">,</span>
            <span class="s2">&quot;name&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">name</span><span class="p">,</span>
            <span class="s2">&quot;number&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">number</span><span class="p">,</span>
            <span class="s2">&quot;wins&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">wins</span><span class="p">,</span>
            <span class="s2">&quot;losses&quot;</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">losses</span><span class="p">,</span>
        <span class="p">}</span>

    <span class="k">def</span> <span class="nf">update</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">dictionary</span><span class="p">):</span>
        <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">dictionary</span><span class="p">:</span>
            <span class="k">if</span> <span class="n">x</span> <span class="o">==</span> <span class="s2">&quot;number&quot;</span><span class="p">:</span>
                <span class="bp">self</span><span class="o">.</span><span class="n">number</span> <span class="o">=</span> <span class="n">dictionary</span><span class="p">[</span><span class="n">x</span><span class="p">]</span>
            <span class="k">if</span> <span class="n">x</span> <span class="o">==</span> <span class="s2">&quot;wins&quot;</span><span class="p">:</span>
                <span class="bp">self</span><span class="o">.</span><span class="n">wins</span> <span class="o">=</span> <span class="n">dictionary</span><span class="p">[</span><span class="n">x</span><span class="p">]</span>
            <span class="k">if</span> <span class="n">x</span> <span class="o">==</span> <span class="s2">&quot;losses&quot;</span><span class="p">:</span>
                <span class="bp">self</span><span class="o">.</span><span class="n">losses</span> <span class="o">=</span> <span class="n">dictionary</span><span class="p">[</span><span class="n">x</span><span class="p">]</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">merge</span><span class="p">(</span><span class="bp">self</span><span class="p">)</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span>
        <span class="k">return</span> <span class="kc">None</span>

    <span class="k">def</span> <span class="nf">delete</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">delete</span><span class="p">(</span><span class="bp">self</span><span class="p">)</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span>
        <span class="k">return</span> <span class="kc">None</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">initUsers</span><span class="p">():</span>
    <span class="k">with</span> <span class="n">app</span><span class="o">.</span><span class="n">app_context</span><span class="p">():</span>
        <span class="n">db</span><span class="o">.</span><span class="n">create_all</span><span class="p">()</span>
        <span class="n">p1</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;Patrick Mahomes&#39;</span><span class="p">,</span> <span class="n">number</span><span class="o">=</span><span class="s1">&#39;15&#39;</span><span class="p">,</span> <span class="n">wins</span><span class="o">=</span><span class="s1">&#39;64&#39;</span><span class="p">,</span> <span class="n">losses</span><span class="o">=</span><span class="s1">&#39;16&#39;</span><span class="p">)</span>
        <span class="n">p2</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;JJ Watt&#39;</span><span class="p">,</span> <span class="n">number</span><span class="o">=</span><span class="s1">&#39;99&#39;</span><span class="p">,</span> <span class="n">wins</span><span class="o">=</span><span class="s1">&#39;77&#39;</span><span class="p">,</span> <span class="n">losses</span><span class="o">=</span><span class="s1">&#39;74&#39;</span><span class="p">)</span>
        <span class="n">p3</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;Russell Wilson&#39;</span><span class="p">,</span> <span class="n">number</span><span class="o">=</span><span class="s1">&#39;3&#39;</span><span class="p">,</span> <span class="n">wins</span><span class="o">=</span><span class="s1">&#39;108&#39;</span><span class="p">,</span> <span class="n">losses</span><span class="o">=</span><span class="s1">&#39;64&#39;</span><span class="p">)</span>
        <span class="n">p4</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;Travis Kelce&#39;</span><span class="p">,</span> <span class="n">number</span><span class="o">=</span><span class="s1">&#39;87&#39;</span><span class="p">,</span> <span class="n">wins</span><span class="o">=</span><span class="s1">&#39;105&#39;</span><span class="p">,</span> <span class="n">losses</span><span class="o">=</span><span class="s1">&#39;39&#39;</span><span class="p">)</span>
        <span class="n">p5</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;Joe Burrow&#39;</span><span class="p">,</span> <span class="n">number</span><span class="o">=</span><span class="s1">&#39;9&#39;</span><span class="p">,</span> <span class="n">wins</span><span class="o">=</span><span class="s1">&#39;24&#39;</span><span class="p">,</span> <span class="n">losses</span><span class="o">=</span><span class="s1">&#39;17&#39;</span><span class="p">)</span>
        <span class="n">p6</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">&#39;Trevor Lawrence&#39;</span><span class="p">,</span> <span class="n">number</span><span class="o">=</span><span class="s1">&#39;16&#39;</span><span class="p">,</span> <span class="n">wins</span><span class="o">=</span><span class="s1">&#39;12&#39;</span><span class="p">,</span> <span class="n">losses</span><span class="o">=</span><span class="s1">&#39;22&#39;</span><span class="p">)</span>


        <span class="n">players</span> <span class="o">=</span> <span class="p">[</span><span class="n">p1</span><span class="p">,</span> <span class="n">p2</span><span class="p">,</span> <span class="n">p3</span><span class="p">,</span> <span class="n">p4</span><span class="p">,</span> <span class="n">p5</span><span class="p">,</span> <span class="n">p6</span><span class="p">]</span>

        <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">players</span><span class="p">:</span>
            <span class="k">try</span><span class="p">:</span>
                <span class="nb">object</span> <span class="o">=</span> <span class="n">x</span><span class="o">.</span><span class="n">create</span><span class="p">()</span>
                <span class="nb">print</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;Created new uid </span><span class="si">{</span><span class="nb">object</span><span class="o">.</span><span class="n">name</span><span class="si">}</span><span class="s2">&quot;</span><span class="p">)</span>
            <span class="k">except</span><span class="p">:</span>
                <span class="nb">print</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;Records exist uid </span><span class="si">{</span><span class="n">x</span><span class="o">.</span><span class="n">name</span><span class="si">}</span><span class="s2">, or error.&quot;</span><span class="p">)</span>
                
<span class="n">initUsers</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>Created new uid Patrick Mahomes
Created new uid JJ Watt
Created new uid Russell Wilson
Created new uid Travis Kelce
Created new uid Joe Burrow
Created new uid Trevor Lawrence
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">find_by_name</span><span class="p">(</span><span class="n">name</span><span class="p">):</span>
    <span class="k">with</span> <span class="n">app</span><span class="o">.</span><span class="n">app_context</span><span class="p">():</span>
        <span class="n">player</span> <span class="o">=</span> <span class="n">Player</span><span class="o">.</span><span class="n">query</span><span class="o">.</span><span class="n">filter_by</span><span class="p">(</span><span class="n">_name</span><span class="o">=</span><span class="n">name</span><span class="p">)</span><span class="o">.</span><span class="n">first</span><span class="p">()</span>
    <span class="k">return</span> <span class="n">player</span> 
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">create</span><span class="p">():</span>
    <span class="n">name</span> <span class="o">=</span> <span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter your name:&quot;</span><span class="p">)</span>
    <span class="n">player</span> <span class="o">=</span> <span class="n">find_by_name</span><span class="p">(</span><span class="n">name</span><span class="p">)</span>
    <span class="k">try</span><span class="p">:</span>
        <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Found</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span> <span class="n">name</span><span class="o">.</span><span class="n">read</span><span class="p">())</span>
        <span class="k">return</span>
    <span class="k">except</span><span class="p">:</span>
        <span class="k">pass</span> 
    
    <span class="n">number</span> <span class="o">=</span> <span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter your number:&quot;</span><span class="p">)</span>
    <span class="n">wins</span> <span class="o">=</span> <span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter your wins:&quot;</span><span class="p">)</span>
    <span class="n">losses</span> <span class="o">=</span> <span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter your losses:&quot;</span><span class="p">)</span>
    
    <span class="n">player</span> <span class="o">=</span> <span class="n">Player</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="n">name</span><span class="p">,</span> 
                <span class="n">number</span><span class="o">=</span><span class="n">number</span><span class="p">,</span> 
                <span class="n">wins</span><span class="o">=</span><span class="n">wins</span><span class="p">,</span>
                <span class="n">losses</span><span class="o">=</span><span class="n">losses</span>
                <span class="p">)</span>
    
    <span class="k">with</span> <span class="n">app</span><span class="o">.</span><span class="n">app_context</span><span class="p">():</span>
        <span class="k">try</span><span class="p">:</span>
            <span class="nb">object</span> <span class="o">=</span> <span class="n">player</span><span class="o">.</span><span class="n">create</span><span class="p">()</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Created</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span> <span class="nb">object</span><span class="o">.</span><span class="n">read</span><span class="p">())</span>
        <span class="k">except</span><span class="p">:</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Unknown error name </span><span class="si">{name}</span><span class="s2">&quot;</span><span class="p">)</span>
        
<span class="n">create</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>Created
 {&#39;id&#39;: 7, &#39;name&#39;: &#39;Kalani&#39;, &#39;number&#39;: 50, &#39;wins&#39;: 60, &#39;losses&#39;: 30}
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">read</span><span class="p">():</span>
    <span class="k">with</span> <span class="n">app</span><span class="o">.</span><span class="n">app_context</span><span class="p">():</span>
        <span class="n">table</span> <span class="o">=</span> <span class="n">Player</span><span class="o">.</span><span class="n">query</span><span class="o">.</span><span class="n">all</span><span class="p">()</span>
    <span class="n">json_ready</span> <span class="o">=</span> <span class="p">[</span><span class="n">player</span><span class="o">.</span><span class="n">read</span><span class="p">()</span> <span class="k">for</span> <span class="n">player</span> <span class="ow">in</span> <span class="n">table</span><span class="p">]</span>
    <span class="k">return</span> <span class="n">json_ready</span>

<span class="n">read</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>[{&#39;id&#39;: 1, &#39;name&#39;: &#39;Patrick Mahomes&#39;, &#39;number&#39;: 15, &#39;wins&#39;: 64, &#39;losses&#39;: 16},
 {&#39;id&#39;: 2, &#39;name&#39;: &#39;JJ Watt&#39;, &#39;number&#39;: 99, &#39;wins&#39;: 77, &#39;losses&#39;: 74},
 {&#39;id&#39;: 3, &#39;name&#39;: &#39;Russell Wilson&#39;, &#39;number&#39;: 3, &#39;wins&#39;: 108, &#39;losses&#39;: 64},
 {&#39;id&#39;: 4, &#39;name&#39;: &#39;Travis Kelce&#39;, &#39;number&#39;: 87, &#39;wins&#39;: 105, &#39;losses&#39;: 39},
 {&#39;id&#39;: 5, &#39;name&#39;: &#39;Joe Burrow&#39;, &#39;number&#39;: 9, &#39;wins&#39;: 24, &#39;losses&#39;: 17},
 {&#39;id&#39;: 6, &#39;name&#39;: &#39;Trevor Lawrence&#39;, &#39;number&#39;: 16, &#39;wins&#39;: 12, &#39;losses&#39;: 22},
 {&#39;id&#39;: 7, &#39;name&#39;: &#39;Kalani&#39;, &#39;number&#39;: 50, &#39;wins&#39;: 60, &#39;losses&#39;: 30}]</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">put</span><span class="p">():</span>
    <span class="n">name</span> <span class="o">=</span> <span class="nb">str</span><span class="p">(</span><span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Who do you want to edit?&quot;</span><span class="p">))</span>
    <span class="n">number</span> <span class="o">=</span> <span class="nb">int</span><span class="p">(</span><span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter the player&#39;s new number or same number&quot;</span><span class="p">))</span>
    <span class="n">wins</span> <span class="o">=</span> <span class="nb">int</span><span class="p">(</span><span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter the player&#39;s new number of wins&quot;</span><span class="p">))</span>
    <span class="n">losses</span> <span class="o">=</span> <span class="nb">int</span><span class="p">(</span><span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Enter the player&#39;s new number of losses&quot;</span><span class="p">))</span>
    <span class="n">body</span> <span class="o">=</span> <span class="p">{</span>
        <span class="s2">&quot;name&quot;</span><span class="p">:</span> <span class="n">name</span><span class="p">,</span>
        <span class="s2">&quot;data&quot;</span><span class="p">:</span> <span class="p">{</span><span class="s2">&quot;number&quot;</span><span class="p">:</span> <span class="n">number</span><span class="p">,</span> <span class="s2">&quot;wins&quot;</span><span class="p">:</span> <span class="n">wins</span><span class="p">,</span> <span class="s2">&quot;losses&quot;</span><span class="p">:</span> <span class="n">losses</span><span class="p">}</span>
    <span class="p">}</span>
    <span class="n">data</span> <span class="o">=</span> <span class="n">body</span><span class="o">.</span><span class="n">get</span><span class="p">(</span><span class="s1">&#39;data&#39;</span><span class="p">)</span>
    <span class="n">player</span> <span class="o">=</span> <span class="n">find_by_name</span><span class="p">(</span><span class="n">name</span><span class="p">)</span>
    <span class="k">with</span> <span class="n">app</span><span class="o">.</span><span class="n">app_context</span><span class="p">():</span>
        <span class="n">player</span><span class="o">.</span><span class="n">update</span><span class="p">(</span><span class="n">data</span><span class="p">)</span>
        <span class="n">db</span><span class="o">.</span><span class="n">session</span><span class="o">.</span><span class="n">commit</span><span class="p">()</span>
    <span class="k">return</span> <span class="sa">f</span><span class="s2">&quot;</span><span class="si">{</span><span class="n">player</span><span class="o">.</span><span class="n">name</span><span class="si">}</span><span class="s2"> at id </span><span class="si">{</span><span class="n">player</span><span class="o">.</span><span class="n">id</span><span class="si">}</span><span class="s2"> has been updated&quot;</span>

<span class="n">put</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>&#39;Kalani at id 7 has been updated&#39;</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">delete</span><span class="p">():</span>
    <span class="n">name</span> <span class="o">=</span> <span class="nb">str</span><span class="p">(</span><span class="nb">input</span><span class="p">(</span><span class="s2">&quot;Who do you want to delete?&quot;</span><span class="p">))</span>
    <span class="n">player</span> <span class="o">=</span> <span class="n">find_by_name</span><span class="p">(</span><span class="n">name</span><span class="p">)</span>
    <span class="k">with</span> <span class="n">app</span><span class="o">.</span><span class="n">app_context</span><span class="p">():</span>
        <span class="n">player</span><span class="o">.</span><span class="n">delete</span><span class="p">()</span>
    <span class="k">return</span> <span class="sa">f</span><span class="s2">&quot;</span><span class="si">{</span><span class="n">player</span><span class="o">.</span><span class="n">name</span><span class="si">}</span><span class="s2"> at id </span><span class="si">{</span><span class="n">player</span><span class="o">.</span><span class="n">id</span><span class="si">}</span><span class="s2"> has been deleted&quot;</span>

<span class="n">delete</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>&#39;Kalani at id 7 has been deleted&#39;</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 

