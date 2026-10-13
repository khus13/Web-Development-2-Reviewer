# Web Dev Cheat Sheet: JavaScript • jQuery • PHP (+ MySQL)

A quick reference for client-side (JS, jQuery) and server-side (PHP) web development.

## Table of Contents
1. [Big Picture](#1-big-picture)
2. [JavaScript](#2-javascript)
3. [jQuery](#3-jquery)
4. [AJAX](#4-ajax)
5. [PHP](#5-php)
6. [PHP + MySQL](#6-php--mysql)
7. [Putting It Together](#7-putting-it-together)

---

## 1. Big Picture

| | JavaScript / jQuery | PHP |
|---|---|---|
| Runs on | **Client** (browser) | **Server** |
| Interpreted by | Browser | PHP engine on the server |
| Client needs | Browser | Browser only (PHP is not installed client-side) |
| File type | `.js` or inside `<script>` | `.php` (can embed HTML) |
| Typical role | UI, events, DOM, effects | Logic, sessions, database access |

**Scripting language** = interpreted at runtime, usually no compile step, often embedded in another environment (e.g., HTML).

**Local PHP setup (XAMPP):** start Apache (and MySQL) → put project folder in `htdocs` → use `.php` extension → open `http://localhost/PROJECT_FOLDER/file.php`.

---

## 2. JavaScript

### Variables & Types
```js
let x = 5;          // reassignable
const PI = 3.14;    // constant
var old = "legacy"; // function-scoped, avoid
```
Types: `string`, `number`, `boolean`, `null`, `undefined`, `object`, `array` (object), `function`.

### Operators
| Type | Operators |
|---|---|
| Arithmetic | `+ - * / % **` |
| Comparison | `==` (loose) `===` (strict) `!= !== < > <= >=` |
| Logical | `&& \|\| !` |
| Assignment | `= += -= *= /= %=` |

### Control Flow
```js
if (a) { } else if (b) { } else { }
switch (v) { case 1: ...; break; default: ...; }
for (let i = 0; i < n; i++) { }
while (cond) { }
for (const item of arr) { }     // values
for (const key in obj) { }      // keys
```

### Functions
```js
function add(a, b) { return a + b; }
const add2 = (a, b) => a + b;   // arrow function
```

### Arrays & Objects
```js
const arr = [1, 2, 3];
arr.push(4); arr.pop(); arr.length;
arr.map(n => n * 2); arr.filter(n => n > 1); arr.forEach(n => {});

const obj = { name: "A", age: 20 };
obj.name; obj["age"];
```

### Strings
`length`, `toUpperCase()`, `toLowerCase()`, `slice()`, `substring()`, `indexOf()`, `replace()`, `split()`, `trim()`; template literal: `` `Hi ${name}` ``

### DOM (plain JS)
```js
document.getElementById("id");
document.querySelector(".cls");       // first match
document.querySelectorAll("li");      // all matches
el.textContent = "text";
el.innerHTML = "<b>html</b>";
el.setAttribute("type", "password");
el.classList.add("a"); el.classList.remove("a"); el.classList.toggle("a");
el.style.color = "red";
```

### Events
```js
btn.addEventListener("click", (e) => { e.preventDefault(); });
```
Common: `click`, `dblclick`, `mouseover`, `mouseout`, `keydown`, `keyup`, `input`, `change`, `submit`, `focus`, `blur`, `load`, `resize`, `scroll`.

### JSON
```js
JSON.stringify(obj);   // object → string
JSON.parse(str);       // string → object
```

---

## 3. jQuery

> "Write less, do more." An open-source JS library that simplifies DOM traversal/manipulation, events, effects, and AJAX with cross-browser consistency.

### Include It
```html
<!-- Download and link locally, or use a CDN -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
```

### Core Syntax
`$()` is shorthand for `jQuery()`. It takes a **CSS-style selector** and returns a jQuery object.
```js
$(document).ready(function () {
  // runs when the DOM is ready
});
```

### Selectors
| Selector | Selects |
|---|---|
| `$("*")` | All elements |
| `$("tag")` | All elements with tag name |
| `$("#id")` | Element with id (single) |
| `$(".class")` / `$("input.class")` | Elements with class |
| `$("li:first")` | First matched element |
| `$("li.link")` | `<li>` elements with class `link` |

### Get / Set Content
| Method | Purpose |
|---|---|
| `.html()` / `.html(val)` | Get (first element) / set inner HTML |
| `.text()` / `.text(val)` | Get / set text |
| `.val()` / `.val(v)` | Get / set form input value |

```js
$("li:first").html();
$("#p2").text(function () { return "New text"; });
```

### Insert / Replace / Remove
| Method | Effect |
|---|---|
| `.before()` | Insert **before** the selected element |
| `.after()` | Insert **after** the selected element |
| `.prepend()` | Insert **inside**, at the start |
| `.append()` | Insert **inside**, at the end |
| `.replaceWith()` | Replace the element with new content |
| `.remove()` | Remove element(s) |

```js
$("ul").append('<li id="append">APPEND</li>');
$("#removeMe").remove();
```

### Attributes & Classes
```js
$el.attr("type");               // get
$el.attr("type", "password");   // set
$el.removeAttr("value");
$el.addClass("cls");            // adds, doesn't overwrite existing
$el.removeClass("cls");
$el.toggleClass("cls");
```

### CSS
```js
$el.css("background-color");                  // get
$el.css("background-color", "blue");          // set one
$el.css("height", "+=80");                    // relative change
$el.css({ "font-size": "30px", color: "pink" }); // set many (object)
```

### Traversing the DOM
| Needs a selector | Description |
|---|---|
| `.find(sel)` | Descendants matching selector |
| `.closest(sel)` | Nearest ancestor matching selector |

| Selector optional | Description |
|---|---|
| `.parent()` / `.parents()` | Direct parent / all ancestors |
| `.children()` | Direct children |
| `.siblings()` | All siblings |
| `.next()` / `.nextAll()` | Next sibling / all following siblings |
| `.prev()` / `.prevAll()` | Previous sibling / all preceding siblings |

### Looping: `.each()`
```js
$("li").each(function () {
  console.log($(this).html());   // $(this) = current element
});
```

### Events: `.on()`
```js
$("#btn").on("click", function () { console.log("clicked"); });
$("#btn").on("click", handlerFn);
```

| Category | Events |
|---|---|
| UI/Element | `focus`, `blur`, `change` |
| Keyboard | `input`, `keydown`, `keyup`, `keypress` |
| Mouse | `click`, `dblclick`, `mouseup`, `mousedown`, `mouseover`, `mousemove`, `mouseout`, `hover` |
| Form | `submit`, `select`, `change` |
| Document | `ready`, `load`, `unload` |
| Browser | `error`, `resize`, `scroll` |

**Event object properties:** `type`, `which` (key/button), `data`, `target`, `pageX`, `pageY`, `timeStamp`
**Event object methods:** `.preventDefault()` (e.g., stop form reload), `.stopPropagation()` (stop bubbling)

### Effects
Duration accepts `"slow"`, `"fast"`, or milliseconds.

| Group | Methods |
|---|---|
| Basic | `.show()` `.hide()` `.toggle()` |
| Fading | `.fadeIn()` `.fadeOut()` `.fadeTo()` `.fadeToggle()` |
| Sliding | `.slideDown()` (show) `.slideUp()` (hide) `.slideToggle()` |
| Custom | `.delay()` `.stop()` `.animate()` |

```js
$("#box").animate(
  { opacity: "0.0", height: "0px", width: "0px" },
  1000,
  function () { $("#box").remove(); }   // callback when done
);
```

---

## 4. AJAX

**Asynchronous JavaScript And XML**: load/send data to a server **without reloading the page**. The user can keep interacting while waiting. Returns **HTML, XML, or JSON** (JSON is the most common).

### How It Works
1. Event occurs (load, click…)
2. `XMLHttpRequest` object created
3. Request sent to server
4. Server processes it
5. Server sends response
6. JavaScript reads response
7. Page updated with the data

### XMLHttpRequest
```js
const xhttp = new XMLHttpRequest();

xhttp.onreadystatechange = function () {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText);
    console.log(data);
  }
};

// GET: request data
xhttp.open("GET", "data.php?id=1&name=Doe", true);
xhttp.send();

// POST: send data
xhttp.open("POST", "save.php", true);
xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhttp.send("id=1&name=Doe");
```

| `.open(method, url, async)` | |
|---|---|
| `method` | `"GET"` or `"POST"` |
| `url` | Server file/location |
| `async` | `true` / `false` |

| Property | Meaning |
|---|---|
| `responseText` / `responseXML` | Returned data (text / XML) |
| `readyState` | `0` not initialized · `1` connected · `2` received · `3` processing · `4` done |
| `status` | `200` OK · `403` Forbidden · `404` Not found |
| `statusText` | Status description |

**Parsing JSON:** `JSON.parse(text)` or `jQuery.parseJSON(text)`.

### jQuery AJAX Shortcut
```js
$.ajax({
  url: "data.php",
  method: "POST",
  data: { id: 1 },
  dataType: "json",
  success: function (res) { console.log(res); },
  error: function (err) { console.log(err); }
});
```

### API
**Application Programming Interface**: a software intermediary that lets two applications talk. It sits between the client application and the server: the app sends a request, the server processes it, and the response comes back in a readable form.

---

## 5. PHP

> **PHP: Hypertext Preprocessor.** Open source, cross-platform, server-side, loosely typed, easy to learn, works with MySQL and other DBs (PostgreSQL, Oracle, MS SQL Server, ODBC).

### Syntax
```php
<?php
  // PHP code here
?>
```
PHP files can contain HTML and JS alongside PHP.

### Output
```php
echo "text", $var;     // multiple args OK, no return value
print "text";          // single arg, always returns 1
```

### Variables
- Start with `$`, case-sensitive
- Start with a letter or `_` (never a number)
- No spaces

```php
$name = "Ana";
define('PI', 3.14);           // constant (value can't change)
```

### Data Types
`string`, `integer` (see `PHP_INT_MAX`), `float`, `boolean`, `array`, `null`
PHP is **loosely typed**: it infers the type from the value.

```php
$c = $a + (int) $b;     // type casting
var_dump($d);           // show type and value
```

### Operators
| Type | Operators |
|---|---|
| Arithmetic | `+ - * / %`, negation `-n`, concatenation `.` |
| Assignment | `= += -= *= /= %= .=` |
| Comparison | `== === != <> !== > < >= <=` and spaceship `<=>` (PHP 7+) |
| Logical | `and` `&&`, `or` `\|\|`, `xor`, `!` |

`==` compares value; `===` compares value **and** type.

### Strings
| Style | Behavior |
|---|---|
| `'single'` | Literal; escape `'` with `\'` |
| `"double"` | Interpolates `$vars`; escape `$` with `\$` |
| Heredoc `<<<EOT … EOT;` | Multi-line, behaves like double quotes |
| Nowdoc `<<<'EOT' … EOT;` | Multi-line, behaves like single quotes (no interpolation) |

```php
$msg = <<<EOT
Hello $name
EOT;
```

**Common string functions**

| Function | Purpose |
|---|---|
| `strtolower()` / `strtoupper()` | Change case |
| `ucfirst()` / `lcfirst()` | Upper/lower the first character |
| `strlen()` | Length (spaces counted) |
| `explode(sep, str)` | String → array |
| `substr(str, start, len)` | Part of a string |
| `str_replace(find, replace, str)` | Find and replace |
| `strpos(str, find)` | Position of substring |
| `str_word_count()` | Count words |
| `md5()` / `sha1()` | Hashes (⚠ **not safe for passwords**; use `password_hash()` / `password_verify()`) |

### Control Structures
```php
if (c1) { } elseif (c2) { } else { }

switch ($x) { case 1: ...; break; default: ...; }

for ($i = 0; $i < 5; $i++) { }
while (cond) { }
foreach ($arr as $value) { }
foreach ($arr as $key => $value) { }
```

### Arrays
```php
$empty = [];  // or array()
$list  = ["a", "b", "c"];                       // numeric (indexed)
$assoc = ["first" => "A", "last" => "B"];       // associative (named keys)
$multi = ["k1" => ["v1", "v2"], "k2" => ["v3"]];// multi-dimensional

echo $list[0];  echo $assoc["first"];  echo $multi["k1"][0];
```

| Function | Purpose |
|---|---|
| `count($a)` | Number of elements |
| `is_array($a)` | Is it an array? |
| `sort($a)` | Sort by value (reindexes) |
| `asort($a)` | Sort by value, **keep keys** |
| `ksort($a)` | Sort by key |
| `array_push($a, v1, v2)` | Add to end |
| `array_splice($a, start, length)` | Remove/replace elements |

### Functions
```php
function greet($name) {
  return "Hi $name";   // return is optional
}
$result = greet("Ana");
```

### Cookies
Small file (max ~4 KB) stored in the **client's** browser; readable only by the issuing domain; user can disable them.

```php
setcookie(name, value, [expiry], [path], [domain], [secure], [httponly]);

setcookie("user", "abc", time() + 3600, "/");  // 1 hour, whole site
echo $_COOKIE["user"];
setcookie("user", "", time() - 3600);          // delete (past expiry)
```
- Must be called **before any HTML output**
- `path "/"` = available on the whole domain
- `secure = true` → sent only over HTTPS
- `httponly = true` → JavaScript cannot read it

### Sessions
Data stored on the **server**, tied to a unique **session ID** (kept in a cookie, or the URL if cookies are off). Holds more data than cookies; ends when the browser closes (store permanent data in a DB).

```php
<?php
session_start();                  // must come before any HTML
$_SESSION["key"] = "value";       // set
if (isset($_SESSION["key"])) { }  // check
unset($_SESSION["key"]);          // remove one item
session_destroy();                // end the whole session
?>
```

| | Cookie | Session |
|---|---|---|
| Stored | Client | Server |
| Size | ~4 KB | Larger |
| Array | `$_COOKIE` | `$_SESSION` |
| Security | Visible to the user | Safer (only the ID is on the client) |

### Handling Form Data
```php
$_GET["field"];      // from the URL query string
$_POST["field"];     // from the request body
$_REQUEST["field"];  // GET + POST + COOKIE
```

---

## 6. PHP + MySQL

### Connect
```php
$con = mysqli_connect($host, $user, $password, $db_name);
// XAMPP default: "localhost", "root", "" (empty password)

if (!$con) { die("Connection failed: " . mysqli_connect_error()); }
```

### Create Database / Table
```php
$con->query("CREATE DATABASE db_name");
$con->query("CREATE TABLE table_name (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50))");
// query() returns true on success, false on failure
```

### Prepared Statements (use these to prevent SQL injection)
```php
$stmt = $con->prepare("SELECT * FROM users WHERE name = ?");
$stmt->bind_param("s", $name);   // bind the parameters
$stmt->execute();
$result = $stmt->get_result();

while ($row = $result->fetch_assoc()) {
  echo $row["name"];
}

$stmt->close();
$con->close();
```

| `bind_param` type | Meaning |
|---|---|
| `s` | string |
| `i` | integer |
| `d` | double/float |
| `b` | blob |

Multiple params: `$stmt->bind_param("si", $name, $age);`

### Common SQL
```sql
SELECT col1, col2 FROM tbl WHERE cond ORDER BY col DESC LIMIT 10;
INSERT INTO tbl (a, b) VALUES (?, ?);
UPDATE tbl SET a = ? WHERE id = ?;
DELETE FROM tbl WHERE id = ?;
```

---

## 7. Putting It Together

**Typical flow: page → AJAX → PHP → MySQL → JSON → page**

**Frontend (jQuery):**
```js
$("#btn").on("click", function () {
  $.ajax({
    url: "get_users.php",
    method: "GET",
    dataType: "json",
    success: function (users) {
      $("#list").empty();
      $.each(users, function (i, u) {
        $("#list").append("<li>" + u.name + "</li>");
      });
    }
  });
});
```

**Backend (`get_users.php`):**
```php
<?php
$con = mysqli_connect("localhost", "root", "", "mydb");
$result = $con->query("SELECT id, name FROM users");
$rows = $result->fetch_all(MYSQLI_ASSOC);
header("Content-Type: application/json");
echo json_encode($rows);
```

### Quick Reminders
- `session_start()` and `setcookie()` go **before** any HTML output.
- Always use **prepared statements** with user input.
- Never trust client-side validation alone; validate again in PHP.
- Use `===` over `==` in both JS and PHP.
- Escape output (`htmlspecialchars()` in PHP) to prevent XSS.
- Use `$(document).ready()` so the DOM exists before jQuery runs.
- Prefer `password_hash()` over `md5()`/`sha1()`.
