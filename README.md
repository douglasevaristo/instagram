# Inserindo fotos do instagram em html/jquery
Passo a passo para adicionar fotos de um perfil do instagram em uma webpage com jquery

1. Criar um app no instagram (https://www.instagram.com/developer/):
Seguir o passo a passo desse tutorial - https://elfsight.com/blog/2016/05/how-to-get-instagram-access-token/

2. Criar o token de acesso a conta:
Na verdade essa etapa está no tutorial anterior. Vale lembrar que esse token deve ser criado da conta do instagram que deseja exibir as fotos.

3. Inserir o fancybox dentro do head:

```javascript
<link type="text/css" rel="stylesheet" href="http://cdnjs.cloudflare.com/ajax/libs/fancybox/2.1.5/jquery.fancybox.min.css"/>
```

4. Inserir o jquery e o fancybox.js antes de fechar o body:

```javascript
<div class="insta">
	<div class="myig_gallery"></div>
</div>
```

5. Inserir o jquery e o fancybox.js antes de fechar o body:

```javascript
<script src="https://code.jquery.com/jquery-2.2.4.min.js" integrity="sha256-BbhdlvQf/xTY9gja0Dq3HiwQF8LaCRTXxZKRutelT44=" crossorigin="anonymous"></script>
<script src="http://cdnjs.cloudflare.com/ajax/libs/fancybox/2.1.5/jquery.fancybox.min.js"></script>
```

6. Inserir o script abaixo fazendo a alteração necessaria no token:

```javascript
	$(document).ready(function(){
		$(".myig_popup").fancybox({
			openEffect : 'fade',
			closeEffect : 'fade'
		});
	});

	$.fn.myig = function(g, h, j) {
	      var k = ($(this).attr("id") != null || $(this).attr("id") != undefined ? '#' + $(this).attr("id") : '.' + $(this).attr("class"));
	
	      myig_gallery(k, "");
	
	        function myig_gallery(e, f) {
	            $.ajax({
	                url: 'https://api.instagram.com/v1/users/' + g + '/media/recent/?access_token=' + j + '&count=' + h + '&max_id=' + f,
	                crossDomain: true,
	                dataType: 'jsonp'
	            }).done(function(c) {
	                var d = '';
	                d += '<section id="photos">';
	                $.each(c.data, function(i, a) {
	                    var b = '';
	                    b += (c.data[i].caption == null || c.data[i].caption == undefined ? Date(c.data[i].created_time) : c.data[i].caption.text + ' - ' + Date(c.data[i].created_time));
	                    d += '  <a href="' + c.data[i].images.standard_resolution.url.replace(/\\/, "") + '" class="myig_popup" rel="myig_popup">';
	                    d += '		<img src="' + c.data[i].images.thumbnail.url.replace(/\\/, "") + '" alt="" title="' + b + '">';
	                    d += '  </a>';
	                });
	                d += '</section>'
	                $(e + ' .myig_gallery').append(d);
	            })
	        }
	}

	$(document).ready(function(){
		$(".insta").myig(
			ins_id = xxxxxx, // esse id da conta pode ser obtido 
			ins_count = 10, // Será o numero de fotos retornadas (maximo de 20)
			ins_token = 'xxxxxxxxxxxxxxxx' // Inserir o token
		);
	});
```
