Tartalom elérése
================

A kívánt tartalom a `setcontent` tag használatával érhető el. Az alábbi példa
egy 'pages' tartalomtípusú és 'about' slug című rekordot fog betölteni.

<pre class="brush: html">
{% setcontent about = 'page/about' %}

{{ print(about) }}
</pre>

A `setcontent` tag több opcióval is használható, a legtöbb ezek közül opcionális.
A legáltalánosabb formája:

<code>
{% setcontent _variable_ = '_contenttype_' %}
</code>

Ez egy megadott _contenttype_ tartalomtípusú rekordot fog a _variable_ változóba
betölteni.

Például a `{% setcontent mypages = 'pages' %}` a `{{ mypages }}` változót
tölti fel a 'pages' összes értékét tartalmazó tömbbel.

Az eredménylista korlátozása
----------------------------

Általában nincs szükség az _összes_ rekord elérésére csupán annak csak egy
részére. A 'where' kulcsszó használatával korlátozhatod a kapott rekordok számát
(bővebben lejjeb), bár gyakran egyszerűbb egy a Bolt által biztosított rövidítést
használni.

Ha egy rekordra van szükség és ismert az id vagy a slug, így is használhatod:

<pre class="brush: html">
{# page betöltése slug 'about' használatával #}
{% setcontent about = 'page/about' %}

{# newsitem elérése id 12 -vel #}
{% setcontent news = 'news/12' %}
</pre>

Ha az 'utolsó 5 pages' vagy az 'első 3 reviews' tételre lenne szükség, arra is
van lehetőség:

<pre class="brush: html">
{% setcontent latestpages = 'pages/latest/5' %}

{{ print(latestpages) }}
</pre>

and:

<pre class="brush: html">
{% setcontent firstreviews = 'reviews/first/3' %}

{{ print(firstreviews) }}
</pre>

A `where` használata
-------------

Ha sokkal részletesebb és pontosabb feltétel szerint szeretnél rekordokat kiválasztani,
használhatod a `where` feltételt. A paramétereket hashként kell felsorolni tehát több
elemből állhat.

<pre class="brush: html">
{# az összes pages amiben a username 'bob' #}
{% setcontent mypages = 'pages' where { username: 'bob' } %}

{# az összes events ahol az eventdate '2012-10-15' #}
{% setcontent myevents = 'events' where { eventdate: '2012-10-15' } %}

</pre>

A fenti példákban a kiválasztott rekordok **egyenlőség** esetén találják meg a
szükséges rekordokat. Van lehetőség 'kisebb vagy engyelő' vagy 'nem egyenlő'
feltételek alkalmazására is.

<pre class="brush: html">
{# az összes pages amiket nem 'bob' hozott létre #}
{% setcontent mypages = 'pages' where { username: '!bob' } %}

{# az összes events amely eventdate mezője '2012-10-15' vagy korábbi #}
{% setcontent myevents = 'events' where { eventdate: '&lt;2012-10-15' } %}

{# a blog bejegyzések amelyek múlt hétfő előtt lettek publikálva #}
{% setcontent myarticles = 'entries' where { status: 'published', datepublish: '&lt; last monday' } %}

{# az összes books tétel amelynél az amountsold nagyobb mint 1,000 #}
{% setcontent mybooks = 'books' where { amountsold: '&gt;1000' } %}
</pre>

<p class="tip"><strong>Tipp:</strong><code>'&lt;=2012-12-01'</code> esetén
Bolt csak azokat a dátumokat választja ki amik kisebb vagy egyenlők <code>'2012-12-01 00:00:00'</code>-nél. Ha azt akarod, hogy December 1 is a beletartozzon, használd a
<code>'&lt;2012-12-02'</code> formulát. </p>

### A `%like%` opció

<pre class="brush: html">
{# az összes oldal ahol a cím 'ipsum'-ot tartalmaz #}
{% setcontent mypages = 'pages' where { title: '%ipsum%' } %}
</pre>

A `%like%` opció kis- és nagybetűre nem érzékeny és a szó határokat nem veszi figyelembe. Tehát a fenti példa visszaadja az oldalakat, amelyek címében az alábbiak vannak:

  - 'Lorum ipsum dolor'
  - 'LORUM IPSUM DOLOR'
  - 'Lorumipsumdolor'
  - 'ipsumdolor'

de ezeket nem:

  - 'Lorum ipsüm dolor'
  - 'Lorum ips um dolor'

<p class="tip"><strong>Tipp:</strong> Amikor a <code>%</code>-ot használod, a Bolt
a mező elejére vagy végére illeszti a keresési mintát. Például:
a <code>'lore%'</code> és <code>'olor%'</code> mindegyike megtalálja a "Lorem Ipsum
Dolor", szöveget de az <code>'ipsu%'</code> nem. </p>

### Taxonómiák használata

Ugyanazt a szintaxist használhatod megadott taxonómiával rendelkező rekordok betöltéséhez. Ne feledd, a lekérdezés felépíthető egy adott taxonómia _többesszámú_ nevéből:

<pre class="brush: html">
{# az összes 'events' a 'music' kategóriában #}
{% setcontent myevents = 'events' where { categories: 'music' } %}

{# az összes 'pages', 'book' vagy 'movie' taggel #}
{% setcontent mypages = 'pages' where { tags: 'book || movie' } %}
</pre>

### Kiválasztás dátum szerint

Több 'hivatkozás' is van amivel egy múlt- vagy jövőbeli dátum szerint szűrhetsz. 
Néhány példa:

  - `now` - A pillanatnyi dátum és időpont.
  - `today` - A jelenlegi dátum, ma éjfélkor.
  - `tomorrow` - A holnapi dátum, éjfélkor.
  - `yesterday` - A tegnapi dátum, éjfélkor.
  - `last year`
  - `next thursday`

A dárum hivatkozásokat így használhatod:

<pre class="brush: html">
{# 'pages' amik tegnap _előtt_ lettek publikálva #}
{% setcontent mypages = 'pages' where { datepublish: '&lt;yesterday' } %}

{# ha a tegnapi napot is tartalmaznia kell, a `where`-ben használj 'before today'-t #}
{% setcontent mypages = 'pages' where { datepublish: '&lt;today' } %}

{# Selecting pages published earlier today, or in the future #}
{% setcontent mypages = 'pages' where { datepublish: '&gt;today' } %}

{# Selecting pages published today only #}
{% setcontent mypages = 'pages' where { datepublish: '&gt;today', datepublish: '&lt;tomorrow' } %}
</pre>

<p class="tip"><strong>Tipp:</strong> Egy dátum mezőre hivatkozó 'where' parancs esetében használhatsz olyan szöveges, relatív dátumokat mint a <code>'last monday'</code>
vagy <code>'&gt; this year'</code>. A Bolt ezeket belül <code>strtotime()</code> függvénnyel konvertálja, így erről részletes információt találsz a <a href="http://php.net/manual/en/function.strtotime.php" target="_blank"> kézikönyvben </a></p>

Ahogy azt korábban említettük, több paramétert is használhatsz egy 'where' szűrőben:

<pre class="brush: html">
{# öszes 'pages' amit nem 'pete' kreált és 2012 júl után, egy .jpg képpel együtt jöt létre #}
{% setcontent mypages = 'pages' where { username: '!pete', datecreated: '>2012-07-31', image: '%.jpg%' } %}
</pre>

### 'AND' és 'OR'

You can use the `&&` and `||`-parameters to select on two criteria for any
field. However, you can't use something like `where { username: '!pete',
username: '!mike'}` because of the way hashes work in twig: The second
`username` would overwrite the first. Instead, you can use the `&&` and
`||`-parameters to either select using `AND` or `OR`. examples:

<pre class="brush: html">
{# get all pages created by 'pete' or 'mike' #}
{% setcontent mypages = 'pages' where { username: 'pete || mike' } %}

{# get all pages with an id greater than 29, but smaller or equal to 37 #}
{% setcontent mypages = 'pages' where { id: '>29 && &lt;=37' } %}
</pre>

Please note that using these operators, it'll be quite easy to create a where
statement that will never give good results:

<pre class="brush: html">
{# This will _always_ match: #}
{% setcontent mypages = 'pages' where { username: '!pete || !mike' } %}

{# This will never work: #}
{% setcontent mypages = 'pages' where { id: '&lt;29 && &gt37' } %}
</pre>

By using the `|||`-operator (three pipes) you can create an OR-part for multiple
columns. For example:

<pre class="brush: html">
{# Select users from Amsterdam that match either username 'pete' or firstname 'Mike' #}
{% setcontent mypages = 'pages' where { city: 'Amsterdam', 'username ||| firstname': 'pete ||| Mike' } %}

{# Query output:
    WHERE ( (city = 'Amsterdam') AND ( (username = 'pete') OR (firstname = 'Mike') ) )
#}
</pre>

Since 'and' is the default, there is no `&&&` equivalent to `|||`.


Using `limit`
-------------

There's no built-in limit to the amount of records returned. It is good practice
to limit the maximum number of records, by adding a `limit` clause.

<pre class="brush: html">
{# get 10 pages created by 'bob' #}
{% setcontent mypages = 'pages' where { username: 'bob' } limit 10 %}
</pre>

Ordering results
----------------

The results can be sorted by any of the fields of the contenttype, using the
`orderby` clause. You can sort either ascending or descending.

<pre class="brush: html">
{# get 10 pages, sorted alphabetically on title #}
{% setcontent mypages = 'pages' limit 10 orderby 'title' %}

{# get the 10 latest modified pages, sorted datechanged descending #}
{% setcontent mypages = 'pages' limit 10 orderby '-datechanged' %}
</pre>

Note that the records are fetched from the database, according to the `orderby`
parameter. If you use `orderby 'title'`, you will get records with titles
starting with 'a', and not just some records, that are sorted after fetching
them from the database.


One record or multiple records?
-------------------------------

Sometimes Bolt will return one record, and sometimes a set of records. What
makes the difference?

<pre class="brush: html">
{% setcontent mypage = 'page/about' %}
{{ mypage }} {# mypage is one record #}

{% setcontent mypages = 'page/latest/5' %}
{% for mypage in mypages %}
  {{ mypage }} {# mypages is an array, that we can loop #}
{% endfor %}
</pre>

Bolt tries to make an assumption about how you want to use it, based on what
you're requesting. By default, an array is returned, unless one of the following
is the case:

  - `{% setcontent foo = 'bar/1' %}` or `{% setcontent foo = 'bar/qux' %}`: When
    requesting one specific record, only one is returned.
  - `{% setcontent foo = 'page' where { .. } %}`: If 'page' is the singular slug
    of the contenttype 'pages', Bolt assumes you only need one.
  - `{% setcontent foo = 'pages' .. returnsingle %}`: If the 'returnsingle' parameter
    is passed, Bolt assumes you only need one result.

If you use `limit 1`, you will get an array with 1 record. Unless, of course,
one of the above criteria was met.


