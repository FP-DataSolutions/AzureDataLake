# Zadanie 5

W ramach zadania należy zarejestrować przy użyciu U-SQL bibliotekę ADLAExt.dll. Następnie policzyć statystki popularności danej technologii na podstawie danych z pliku Posts.xml.

Przykładowe dane z pliku.

```xml
<?xml version="1.0" encoding="utf-8"?>
<posts>
  <row Id="1" PostTypeId="1" AcceptedAnswerId="13" CreationDate="2010-09-01T19:34:48.000" Score="100" ViewCount="27044" Body="&lt;p&gt;A coworker of mine believes that &lt;em&gt;any&lt;/em&gt; use of in-code comments (ie, not javadoc style method or class comments) is a &lt;a href=&quot;http://en.wikipedia.org/wiki/Code_smell&quot;&gt;code smell&lt;/a&gt;.  What do you think?&lt;/p&gt;&#xA;" OwnerUserId="6" LastEditorUserId="226" LastEditDate="2011-11-25T22:32:41.300" LastActivityDate="2012-11-27T19:29:27.740" Title="&quot;Comments are a code smell&quot;" Tags="&lt;comments&gt;&lt;anti-patterns&gt;" AnswerCount="34" CommentCount="10" FavoriteCount="49" ClosedDate="2012-11-27T20:11:51.580" CommunityOwnedDate="2011-01-31T09:04:54.130" />
  <row Id="3" PostTypeId="2" ParentId="1" CreationDate="2010-09-01T19:36:50.053" Score="29" Body="&lt;p&gt;Ideally, code should be so well coded that it should be auto explicative. In the real world, we know that also very high quality code needs sometimes commenting.&lt;/p&gt;&#xA;&#xA;&lt;p&gt;What you should absolutely avoid is &quot;comment-code redundancy&quot; (comments that don't add anything to code):&lt;/p&gt;&#xA;&#xA;&lt;pre&gt;&lt;code&gt;i++; // Increment i by 1&#xA;&lt;/code&gt;&lt;/pre&gt;&#xA;&#xA;&lt;p&gt;Then, if there's a good (and maintained/aligned) code design and documentation, commenting is even less useful.&lt;/p&gt;&#xA;&#xA;&lt;p&gt;But in some circumstances comments can be a good aid in code readability:&lt;/p&gt;&#xA;&#xA;&lt;pre&gt;&lt;code&gt;while( foo )&#xA;{&#xA;     if( dummy )&#xA;     {&#xA;     }&#xA;     else // !dummy&#xA;     {&#xA;     }&#xA;} // end while( foo )&#xA;&lt;/code&gt;&lt;/pre&gt;&#xA;&#xA;&lt;p&gt;Don't forget that you have to maintain and keep in sync also comments... outdated or wrong comments can be a terrible pain! And, as a general rule, commenting too much can be a symptom of bad programming.&lt;/p&gt;&#xA;" OwnerUserId="11" LastEditorUserId="11" LastEditDate="2010-09-01T20:41:14.273" LastActivityDate="2010-09-01T20:41:14.273" CommentCount="17" CommunityOwnedDate="2011-01-31T09:04:54.130" />
  <row Id="201034" PostTypeId="2" ParentId="200989" CreationDate="2013-06-10T12:59:41.723" Score="1" Body="&lt;p&gt;I often use that distinction between software and physical things when describing why building software is difficult. &lt;/p&gt;&#xA;&#xA;&lt;p&gt;Users just think it's this woolly thing inside the computer, you can't touch &quot;it&quot;, they don't understand why it breaks etc.&lt;/p&gt;&#xA;&#xA;&lt;p&gt;Salesmen think they can just promise the world to a client and you the developer(s) will be be able to throw it together without too much thought (or too much change to the timescale!)&lt;/p&gt;&#xA;&#xA;&lt;p&gt;So I'd say a major difference is the people involved. In software it can be very difficult to tie people down to agreeing a specification, or simply not deviating from that specification over time.&lt;/p&gt;&#xA;&#xA;&lt;p&gt;Ideally, their would be not much difference between the two, but because your software just lives behind the monitor, they don't realise how big a change something is if you were having to, for example, saw the legs off a table to replace with new awesome legs 2.0&lt;/p&gt;&#xA;" OwnerUserId="14363" LastActivityDate="2013-06-10T12:59:41.723" CommentCount="0" />
  <row Id="201039" PostTypeId="2" ParentId="200980" CreationDate="2013-06-10T13:20:48.523" Score="-1" Body="&lt;p&gt;One of popular solutions, is to break code into modules, here is how it's done in JavaScript:&lt;/p&gt;&#xA;&#xA;&lt;pre&gt;&lt;code&gt;    media.podcast = (function(name) {&#xA;    var fileExtension = 'mp3';        &#xA;&#xA;     function determineFileExtension() {&#xA;         console.log('File extension is of type ' + fileExtension);&#xA;     }&#xA;&#xA;     return {&#xA;         download: function(episode) {&#xA;            console.log('Downloading ' + episode + ' of ' + name);&#xA;            determineFileExtension();&#xA;        }&#xA;    }    &#xA;}('Astronomy podcast'));&#xA;&lt;/code&gt;&lt;/pre&gt;&#xA;&#xA;&lt;p&gt;The &lt;a href=&quot;http://elegantcode.com/2011/02/15/basic-javascript-part-10-the-module-pattern/&quot; rel=&quot;nofollow&quot;&gt;full article explaining this pattern in JavaScript&lt;/a&gt;, apart from that there is number of other ways to define a module, such as &lt;a href=&quot;http://requirejs.org/docs/api.html#defsimple&quot; rel=&quot;nofollow&quot;&gt;RequireJS&lt;/a&gt;, &lt;a href=&quot;http://wiki.commonjs.org/wiki/Modules/1.1&quot; rel=&quot;nofollow&quot;&gt;CommonJS&lt;/a&gt;, Google Closure. Another example is Erlang, where you have both &lt;a href=&quot;http://erlang.org/doc/reference_manual/modules.html&quot; rel=&quot;nofollow&quot;&gt;modules&lt;/a&gt; and &lt;a href=&quot;http://www.erlang.org/doc/design_principles/des_princ.html&quot; rel=&quot;nofollow&quot;&gt;behaviours&lt;/a&gt; that enforce API and pattern, playing similar role as Interfaces in OOP.&lt;/p&gt;&#xA;" OwnerUserId="84352" LastActivityDate="2013-06-10T13:20:48.523" CommentCount="0" />
 </posts>
```

W tym celu należy wykorzystać (uzupełnić poniższy skrypt U-SQL)

```mssql
//Add code
USING XmlXPathProcessor = ADLAExt.Processors.XmlXPathProcessor;
USING StackOverflowCategoryProducer = ADLAExt.Processors.StackOverflowCategoryProducer;
//DECLARE @inputFile string = @"mySamples/StackOverflow/Posts.xml";
//Set proper path
DECLARE @inputFile string = @"Posts.xml";

//Load input file
@ds =
    EXTRACT content string
    FROM @inputFile
    USING Extractors.Text(delimiter:'\r',silent:true);

//extract item from line 
//<row Id="1" PostTypeId="1" AcceptedAnswerId="13" CreationDate="2010-09-01T19:34:48.000" Score="100" ViewCount="27044" Body="&lt;p&gt;A coworker of mine believes that &lt;em&gt;any&lt;/em&gt; use of in-code comments (ie, not javadoc style method or class comments) is a &lt;a href=&quot;http://en.wikipedia.org/wiki/Code_smell&quot;&gt;code smell&lt;/a&gt;.  What do you think?&lt;/p&gt;&#xA;" OwnerUserId="6" LastEditorUserId="226" LastEditDate="2011-11-25T22:32:41.300" LastActivityDate="2012-11-27T19:29:27.740" Title="&quot;Comments are a code smell&quot;" Tags="&lt;comments&gt;&lt;anti-patterns&gt;" AnswerCount="34" CommentCount="10" FavoriteCount="49" ClosedDate="2012-11-27T20:11:51.580" CommunityOwnedDate="2011-01-31T09:04:54.130" />
 
@ds =
    PROCESS @ds
    PRODUCE content ,
            Tags string,
            Id string,
            ViewCount string,
            CreationDate string,
            Title string,
            AnswerCount string,
            OwnerUserId string
            READONLY content
    USING new XmlXPathProcessor( xPathQuery : "/row", columnPaths : new SQL.MAP<string, string>
              {
              {"Id", "Id"},
              {"CreationDate", "CreationDate"},
              {"ViewCount", "ViewCount"},
              {"Tags", "Tags"},
              {"Title", "Title"},
              {"AnswerCount", "AnswerCount"},
              {"OwnerUserId", "OwnerUserId"},
              }
              );
//Filter items Id!=empty
@ds =
    SELECT Tags,
           Id,
           ViewCount,
           CreationDate,
           Title,
           AnswerCount,
           OwnerUserId
    FROM @ds
    WHERE Id != string.Empty;

//Convert types 
//Extract tags Tags="<comments><anti-patterns>"
@postTags =
    SELECT Int32.Parse(p.Id) AS Id,
           string.IsNullOrEmpty(p.ViewCount) ? 0 : Int32.Parse(p.ViewCount) AS ViewCount,
           DateTime.Parse(p.CreationDate) AS CreationDate,
           Title,
           string.IsNullOrEmpty(OwnerUserId) ? 0 : Int32.Parse(OwnerUserId) AS UserId,
           string.IsNullOrEmpty(p.AnswerCount) ? 0 : Int32.Parse(p.AnswerCount) AS AnswerCount,
           new SQL.ARRAY<string>(p.Tags.Split('>').Select(t => t.Replace("<", ""))) AS tags
    FROM @ds AS p;

//Explode tags 
//Eech row -one tags
@postWithSimpleTags =
    SELECT pt.Id,
           pt.ViewCount,
           Tag,
           CreationDate.Year AS Year,
           CreationDate.Month AS Month,
           CreationDate,
           Title,
           UserId,
           AnswerCount,
           "category" AS Category
    FROM @postTags AS pt
         CROSS APPLY
             EXPLODE(tags) AS posttags(Tag);

//Merge category 
@postsByCategory =
    PROCESS @postWithSimpleTags
    PRODUCE Tag,
            Category,
            Id,
            ViewCount,
            Year,
            Month,
            CreationDate,
            Title,
            UserId,
            AnswerCount
    READONLY Id,
             ViewCount,
             Year,
             Month,
             CreationDate,
             Title,
             UserId,
             AnswerCount
    USING new StackOverflowCategoryProducer();

//Compute Statistics
//Add code 

```

 