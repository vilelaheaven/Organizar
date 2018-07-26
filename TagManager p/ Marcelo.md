---


---

<p>Sobre o conceito de eventos no Google Analytics:</p>
<p><a href="#gambiarra-feelings">Se quiser pular direto para a gambiarra em CSS, clique aqui.</a></p>
<p>Existe atualmente uma coisa chamada <strong>Enhanced Commerce</strong> no Analytics, supostamente ele deveria funcionar bem com o Magento, se bem configurado.<br>
<a href="https://docs.magento.com/m1/ee/user_guide/marketing/google-universal-analytics-enhanced-ecommerce.html">Veja aqui<br>
</a></p>
<blockquote>
<p>Isso é ativado em nas “Configurações de comércio eletrônico” no Admin do Google Analytics, depois ativando o “comércio eletrônico básico” e os “produtos relacionados” depois avançando, e você verá a opção “Ativar relatórios avançados de comércio eletrônico”;</p>
</blockquote>
<p><a href="https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce">Aqui tem uma documentação completa sobre isso.</a></p>
<p>Abaixo alguns exemplos de como enviar esses comandos via DataLayer pelo GTM:</p>
<h3 id="adicionar-e-remover-do-carrinho">Adicionar e Remover do carrinho</h3>
<blockquote>
<p>reconhecendo quando uma pessoa tira ou coloca produto no carrinho</p>
</blockquote>
<p>OBS: Você irá usar como gatilho no GTM uma simples pageview (pode limitar a página de checkout, por exemplo. Siga essa lógica pros outros exemplos.</p>
<p><strong>adicionando.js</strong>:</p>
<pre><code>&lt;script&gt;
    dataLayer.push({
      'event': 'addToCart',
      'ecommerce': {
        'currencyCode': 'BRL',
        'add': {
          'products': [{
              'name': 'CANECA HERMIONE',
              'id': '67890',
              'price': '19.90',
              'brand': 'Warner',
              'category': 'Canecas',
              'variant': 'Branca',
          }]
        }
      }
  });
&lt;/script&gt;
</code></pre>
<p><strong>removendo.js</strong></p>
<pre><code>&lt;script&gt;
    dataLayer.push({
      'event': 'removeFromCart',
      'ecommerce': {
        'currencyCode': 'BRL',
        'add': {
          'products': [{
              'name': 'CANECA HERMIONE',
              'id': '67890',
              'price': '19.90',
              'brand': 'Warner',
              'category': 'Canecas',
              'variant': 'Branca',
          }]
        }
      }
  });
&lt;/script&gt;
</code></pre>
<h3 id="clique-em-produtos">Clique em produtos</h3>
<blockquote>
<p>coloque aonde quiser</p>
</blockquote>
<p><strong>Exemplo de um .HTML:</strong></p>
<pre><code>&lt;div class="products"&gt;
    &lt;div class="row"&gt;
        &lt;div class="col-md-3"&gt;
            &lt;div class="ibox"&gt;
                ...
                 &lt;a href="/ecommerce_product_detail.html" data-name="CANECA MINI DC COMICS WONDER WOMAN BODY" data-id="12345" data-price="10.00" data-brand="DC Comics" data-category="Canecas" data-variant="Branca" data-list="Mais visitados" data-position="1" class="btn btn-xs btn-outline btn-primary"&gt;COMPRAR AGORA! &lt;i class="fa fa-long-arrow-right"&gt;&lt;/i&gt; &lt;/a&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class="col-md-3"&gt;
            &lt;div class="ibox"&gt;
                ...
                 &lt;a href="/ecommerce_product_detail.html" data-name="CANECA HERMIONE" data-id="67890" data-price="19.90" data-brand="Warner" data-category="Canecas" data-variant="Branca" data-list="Mais visitados" data-position="2" class="btn btn-xs btn-outline btn-primary"&gt;COMPRAR AGORA! &lt;i class="fa fa-long-arrow-right"&gt;&lt;/i&gt; &lt;/a&gt;
            &lt;/div&gt;
        &lt;/div&gt;

    &lt;/div&gt;
&lt;/div&gt;
</code></pre>
<p><strong>Agora exemplo do script funcionando em cima do html acima</strong></p>
<pre><code>metricasEC = (function(){
    productClick = function(e){
        dataLayer.push({
            'event': 'productClick',
    	    'ecommerce': {
          	    'click': {
                    'actionField': {'list': e.getAttribute("data-list")},
                	'products': [{
                  	'name': e.getAttribute("data-name"),
        	          'id':	e.getAttribute("data-id"),
          	        'price': e.getAttribute("data-price"),
            	      'brand': e.getAttribute("data-brand"),
              	    'category': e.getAttribute("data-category"),
                	  'variant': e.getAttribute("data-variant"),
                  	'position': e.getAttribute("data-position")
                 	}]
               	}
            },
    	});
    }
      return {
      	productClick: productClick
      }
})();
</code></pre>
<h2 id="impressão-de-detalhe-do-produto">Impressão de detalhe do produto</h2>
<p>A impressão de detalhe de produto utiliza a ação  <a href="https://developers.google.com/tag-manager/enhanced-ecommerce#details"><strong>detail</strong></a>  com um ou mais  <strong>productFieldObjects</strong>  e deverá estar presente na página de produto.</p>
<pre><code>&lt;script&gt;
    dataLayer.push({
      'ecommerce': {
        'detail': {
          'actionField': {'list': 'Página de produto'},
          'products': [{
              'name': 'CANECA HERMIONE',
              'id': '67890',
              'price': '19.90',
              'brand': 'Warner',
              'category': 'Canecas',
              'variant': 'Branca',
          }]
         }
       }
    });
&lt;/script&gt;
</code></pre>
<h2 id="impressões-de-promoções">Impressões de promoções</h2>
<p>A visualização das promoções é representado  <a href="https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce?hl=pt-br#promotion-data"><strong>promoFieldObject</strong></a>, descrevendo as promoções internas apresentadas na página, banners, sliders e etc. Esse também é um objeto relacionado ao carregamento da página, lembre-se de o adiciona-lo antes da implementação do GTM.</p>
<pre><code>&lt;script&gt;
    dataLayer.push({
      'ecommerce': {
        'promoView': {
          'promotions': [
           {
             'id': 'JUNE_PROMO13', //ID ou NAME são obrigatórios.
             'name': 'Frete grátis', //Nome da campanha interna
             'creative': 'banner1.jpg', // Nome do arquivo o url do mesmo, serve para medir que arte teve mais impacto
             'position': 'slider1' // Pode ser desde uma área do site ou até mesmo uma posição em um slider
           }]
        }
      }
    });
&lt;/script&gt;

</code></pre>
<h2 id="clique-em-promoções">Clique em promoções</h2>
<p>O click em promoções manda para dentro do dataLayer a ação  <strong>promoClick</strong>  contendo um  <strong>promoFieldObject</strong>  no mesmo, para esse exemplo costumo usar o mesmo modelo de data-attributes passando as informações dentro do link da promoção. Algo importante de se lembrar é que essas promoções são somente internas, adsenses e qualquer outra campanha externa é mensurado em suas respectivas ferramentas.</p>
<p>Nesse exemplo também faço a divisão da view aonde passo os data-attributes do script que dispara o mesmo. Deixei também um exemplo no  <a href="https://jsfiddle.net/bxvxcmrm/1/">jsfiddle</a>que mostra o click funcionando.</p>
<p><strong>slider.html</strong></p>
<pre><code>&lt;div class="promotionSlider"&gt;
    &lt;div class="row"&gt;
        &lt;div class="col-md-3"&gt;
            ...
             &lt;a href="#" onClick="metricasEC.promotionClick(this)" data-name="CANECA MINI DC COMICS WONDER WOMAN BODY" data-id="12345" data-creative="banner1.jpg" data-position="slider1" class="btn btn-xs btn-outline btn-primary"&gt;COMPRAR AGORA! &lt;i class="fa fa-long-arrow-right"&gt;&lt;/i&gt; &lt;/a&gt;
        &lt;/div&gt;
    &lt;/div&gt;
&lt;/div&gt;

</code></pre>
<p><strong>metricaec.js</strong></p>
<pre><code>metricaec = (function(){
    promotionClick = function(e){
        dataLayer.push({
            'event': 'promotionClick',
            'ecommerce': {
                'promoClick': {
                    'promotions': [
                        {
                            'id': e.getAttribute("data-id"),
                            'name': e.getAttribute("data-name"),
                            'creative': e.getAttribute("data-creative"),
                            'position': e.getAttribute("data-position")
                        }]
                    }
                }
            }
            return {
                promotionClick: promotionClick
            }
        });
})();

</code></pre>
<h2 id="section"></h2>
<p>Passos do Checkout</p>
<p>Para entender melhor a implementação dos passos do checkout precisamos separar duas coisas dentro desse passo, carregamento do passo do Checkout da possibilidade do usuário adicionar alguma opção a um passo (Exemplo: Metodo de entrega, meio de pagamento e etc)</p>
<h3 id="carregamento-do-passo-do-checkout">Carregamento do passo do Checkout</h3>
<p>composto da ação  <strong>checkout</strong>  e do  <strong>productFieldObjects</strong>  dentro do actionField.</p>
<pre><code>    &lt;script&gt;
    function onCheckout() {
        dataLayer.push({
            'event': 'checkout',
            'ecommerce': {
                'checkout': {
                    'actionField': {'step': 1},
                    'products': [{
                        'name': 'CANECA HERMIONE',
                        'id': '67890',
                        'price': '19.90',
                        'brand': 'Warner',
                        'category': 'Canecas',
                        'variant': 'Branca',
                    }]
                }
            }
        });
    };
&lt;/script&gt;

</code></pre>
<p>Essa função será disparada toda vez que algum botão leve o usuário para próxima etapa do checkout, ou houver um carregamento de uma etapa do checkout em uma URL. O campo step deverá ser obrigatóriamente um número, o nome do passo é configurado dentro do GTM.</p>
<h3 id="adicionando-opções-dos-usuários-dentro-do-objeto-checkout">Adicionando opções dos usuários dentro do objeto checkout</h3>
<p>A primeira etapa do seu checkout é uma página de identificação, nela o usuário pode se logar no seu sitema, ou fazer um cadastro para continuar no processo de compra, beleza?</p>
<p>Pensando nessa tela, imagine que a opção que esse usuário escolher é que vai vir a ser a opção que passaremos para o checkout. Agora ficou fácil né?</p>
<pre><code>&lt;script&gt;
    function onCheckoutOption(step, checkoutOption) {
        dataLayer.push({
            'event': 'checkoutOption',
            'ecommerce': {
                'checkout_option': {
                    'actionField': {'step': step, 'option': checkoutOption}
                }
            }
        });
    }
&lt;/script&gt;

</code></pre>
<p>Ou seja a cada escolha que o usuário fizer, dentro do click ou do option selecionado deverá ser disparada a função acima</p>
<h3 id="configurando-as-etapas-acima-do-checkout-dentro-do-google-analytics">Configurando as etapas ACIMA do checkout dentro do Google Analytics</h3>
<p>Lembra que falamos que os valores passados no campo ‘step’ deverá ser sempre um número?Agora vamos transformar eles em nomes para as etapas:</p>
<ol>
<li>Selecione a vista de propriedade na qual o enhanced commerce está ativado;</li>
<li>Na parte de administração do seu Google Analytics clique em “Configurações de comércio eletrônico”;</li>
<li>Em etapas do funil do funil, adicione as etapas de acordo com os numéros que você coloco no código;</li>
<li>Clique em salvar e foi.</li>
</ol>
<h2 id="compras">Compras</h2>
<p>ultima tag para implementação do Enhance Commerce no Google Analytics, não menos importante a tag de compra é composta pela ação  <a href="https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce?hl=pt-br#action-data"><strong>purchase</strong></a>  e  <strong>productFieldObjects</strong>.</p>
<pre><code>&lt;script&gt;
    dataLayer.push({
        'ecommerce': {
            'purchase': {
                'actionField': {
                    'id': 'T12345', // ID da transação esse campo é obrigatório
                    'affiliation': 'Online Store', // A loja ou a afiliação da venda
                    'revenue': '35.43', // Valor total da transação incluindo todas as taxas
                    'tax':'4.90', // Taxas imbutidas na transação
                    'shipping': '5.99', // Valor do frete
                    'coupon': 'SUMMER_SALE' // Cupon usado pelo usuário na transação
                },
                'products': [{ // Array dos produtos comprados na transação
                    'name': 'CANECA HERMIONE',
                    'id': '67890',
                    'price': '19.90',
                    'brand': 'Warner',
                    'category': 'Canecas',
                    'variant': 'Branca',
                    'quantity': 1, // Quantidade comprada na transação
                    'coupon': '' // Caso não haja um cupon espefico para o produto passar uma string vazio ou não passar o mesmo dentro do objeto
                }]
            }
        }
    });
&lt;/script&gt;

</code></pre>
<h1 id="gambiarra-feelings">GAMBIARRA FEELINGS:</h1>
<p><em>Mas como todo open-source cada loja é uma história e pode ser uma dor de cabeça ir, vamos pra gambiarra rs</em></p>
<p>Uma forma básica, mas nada estruturada (porém funcional) de habilitar os eventos é ativar eles por <strong>CSS</strong>, foi o que fiz no da Maxibel na maioria dos casos, por não ter acesso a estrutura do Magento e ter que fazer tudo pelo FrontEnd.</p>
<blockquote>
<p>Written by Vilela in <a href="https://github.com/vilelaheaven/stackedit">StackEdit</a>.</p>
</blockquote>

