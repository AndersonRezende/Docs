# Abstract Factory
O padrão Abstract Factory permite que você produza famílias de objetos relacionados sem ter que especificar suas classes concretas.

## Problema
Suponha que você precise criar vários componentes que, embora sejam diferentes um do outro, estejam relacionados a um mesmo grupo. 
Exemplo: digamos que você possua três conjunto de pratos, copos e talheres. Um do tipo clássico, outro do tipo moderno e outro futurista. Não faz sentido misturar itens de um tipo com outro (pratos clássicos, copos modernos e talheres futuristas), porém ambos possuem funcionalidades semelhantes.
Precisamos de uma forma para conseguir reuinir itens do mesmo grupo e apresentá-los conforme necessário.


## Solução
O padrão Abstract Factory sugere declarar interfaces para cada tipo de produto distinto (pratos, copos e talheres no exemplo). Com isso, podemos fazer as variantes (clássico, moderno e futurista no exemplo) seguirem o modelo da interface para possuirem os mesmos comportamentos, porém com caracteristicas próprias.
Exemplo: todos os copos podem implementar uma interface em comum que possua o comportamento encher e esvaziar, porém no modelo futurista podemos definir uma caracteristica para ele saber se encher sozinho enquanto nos modelos clássicos e modernos ele necessite de alguém para realizar a ação.

O próximo passo é declarar uma interface de fábrica abstrata que possua declarações de como criar produtos que façam parte de uma família. Esses métodos devem retornar tipos abstratos de produtos representados por interfaces extraidas previamente.


## Exemplo em código
Será utilizado no exemplo a criação de componentes de interface gráfica (GUI) para os ambientes gráficos Gnome e KDE. Cada um desses possui sua própria implementação de componentes, mas utilizam uma interface comum.
### Interfaces para produtos abstratos
Essas interfaces definem os componentes que serão criados pelas fábricas. Cada fábrica criará uma versão específica destes componentes.
```
interface Janela
{
	public function abrir(): void;
}
```

```
interface Botao
{
	public function clicar(): void;
}
```

### Implementação concreta dos produtos GNOME e KDE
Aqui estão as implementações concretas dos produtos Gnome.
```
class JanelaGnome implements Janela
{
	public function abrir(): void
	{
		echo "Janela GNOME abrindo";
	}
}
```

```
class BotaoGnome implements Botao
{
	public function clicar(): void
	{
		echo "Botão GNOME clicado";
	}
}

```

```
class JanelaKde implements Janela
{
	public function abrir(): void
	{
		echo "Janela KDE abrindo";
	}
}
```

```
class BotaoKde implements Botao
{
	public function clicar(): void
	{
		echo "Botão KDE clicado";
	}
}
```

### Interface da fábrica abstrata
A fábrica abstrata define os métodos para criar os diferentes componentes da interface gráfica.
```
interface GUIFactory
{
	public function criarBotao(): Botao;

	public function criarJanela(): Janela;
}
```

### Implementação das fábricas concretas
Cada fábrica concreta cria produtos correspondentes ao seu ambiente específico, ou seja, Gnome ou KDE.
```
class GnomeFactory implements GUIFactory
{
	public function criarBotao(): Botao
	{
		return new BotaoGnome();
	}

	public function criarJanela(): Janela
	{
		return new JanelaGnome();
	}
}
```

```
class KdeFactory implements GUIFactory
{
	public function criarBotao(): Botao
	{
		return new BotaoKde();
	}

	public function criarJanela(): Janela
	{
		return new JanelaKde();
	}
}
```

### Cliente
O cliente utiliza a fábrica abstrata para criar e manipular os componentes de forma genérica, sem precisar saber de qual fábrica concreta ele está utilizando (GNOME ou KDE).
```
class Aplicacao
{
	private Janela $janela;
	private Botao $botao;

	public function __construct(GUIFactory $factory)
	{
		$this->janela = $factory->criarJanela();
		$this->botao = $factory->criarBotao();
	}

	public function renderizarInterface()
	{
		$this->janela->abrir();
		$this->botao->clicar();
	}
}
```

### Utilizando
```
<?php
$factory1 = new GnomeFactory();
$aplicacao1 = new Aplicacao($factory1);
$aplicacao1->renderizarInterface();

$factory2 = new KdeFactory();
$aplicacao2 = new Aplicacao($factory2);
$aplicacao2->renderizarInterface();

```