
Змінні
---------

Змінні дозволяють розмістити значення, що постійно використовуються в одному місці, 
та повторно використовувати їх у файлі стилів, що дозволяє робити глобальні зміни, 
редактуючи лише один рядок коду.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example">
  <code>// LESS

@color: #4D926F;

#header {
  color: @color;
}
h2 {
  color: @color;
}</code></pre>
  </td><td>
  <pre class="css-output"><code>/* Скопмільований CSS */

#header {
  color: #4D926F;
}
h2 {
  color: #4D926F;
}</code></pre></td>
  </tr>
</table>

Mixin'и
------

Mixin'и дозволяють включати всі властивості одного класу в інший, просто 
вказавши назву класу, як властивість іншого. Вони подібні до змінних які прив’язані 
до цілого класу. Mixin'и можуть також використовуватися як функції та приймати 
аргументи, як видно з прикладу нижче:

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example"><code>// LESS

.rounded-corners (@radius: 5px) {
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
  -ms-border-radius: @radius;
  -o-border-radius: @radius;
  border-radius: @radius;
}

#header {
  .rounded-corners;
}
#footer {
  .rounded-corners(10px);
}</code></pre></td>

<td>
  <pre class="css-output"><code>/* Compiled CSS */

#header {
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  -ms-border-radius: 5px;
  -o-border-radius: 5px;
  border-radius: 5px;
}
#footer {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  -o-border-radius: 10px;
  border-radius: 10px;
}</code></pre>
  </td></tr>
</table>

Вкладені правила
------------

Замість використовувати довгі імена селекторів для того, щоб вказати на наслідування, 
в Less можна просто вкласти селектори всередині інших. Це дозволяє зробити 
наслідування селекторів більш читабельним, а таблиці стилів менших розмірів.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example">
<code>// LESS

#header {
  h1 {
    font-size: 26px;
    font-weight: bold;
  }
  p { font-size: 12px;
    a { text-decoration: none;
      &:hover { border-width: 1px }
    }
  }
}

</code></pre></td>

<td>
  <pre class="css-output"><code>/* Скомпільований CSS */

#header h1 {
  font-size: 26px;
  font-weight: bold;
}
#header p {
  font-size: 12px;
}
#header p a {
  text-decoration: none;
}
#header p a:hover {
  border-width: 1px;
}

</code></pre>
  </td></tr>
</table>

Функції та оператори
----------------------

Чи є деякі елементи у ваших стилях пропорційними відносно інших?
Операції дозволять вам додавати, віднімати, ділити та множити значення 
властивостей та кольори, надаючи вам можливість створювати залежності між властивостями.
Функції використовуються так само, як і в JavaScript коді, дозволяючи як завгодно 
маніпулювати змінними.

<table class="code-example" cellpadding="0">
  <tr><td>
  <pre class="less-example">
<code>// LESS

@the-border: 1px;
@base-color: #111;
@red:        #842210;

#header {
  color: (@base-color * 3);
  border-left: @the-border;
  border-right: (@the-border * 2);
}
#footer {
  color: (@base-color + #003300);
  border-color: desaturate(@red, 10%);
}

</code></pre></td>

<td>
  <pre class="css-output"><code>/* Скомпільований CSS */

#header {
  color: #333;
  border-left: 1px;
  border-right: 2px;
}
#footer {
  color: #114411;
  border-color: #7d2717;
}

</code></pre>
  </td></tr>
</table>

