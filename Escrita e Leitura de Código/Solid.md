# SOLID

É um acrônimo criado por Michael Feathers para representar os postulados apresentados por Robert C. Martin (uncle Bob) no artigo "Postulados de Projeto e Padrões de Projeto".

## Objetivo
São princípios que ajudam o programador a escrever código mais limpos, separando responsabilidades, diminuindo acoplamentos, facilitando na refatoração e ampliando o reaproveitamento de código.
Os princípios do SOLID compreendem 5 diretrizes essenciais, cada uma abordando aspectos específicos do design de software que podem impactar diretamente na manutenibilidade, na extensibilidade e reutilizabilidade.

## SOLID
<ol>
	<li>S: Single Responsiblity Principle - Princípio da responsabilidade única</li>
	<li>O: Open-Closed Principle - Princípio aberto-fechado</li>
	<li>L: Liskov Substitution Principle - Princípio da substituição de Liskov</li>
	<li>I: Interface Segregation Principle - Princípio da segregação de interface</li>
	<li>D: Dependency Inversion Principle - Princípio da inversão de dependência</li>
</ol>

## Na prática

### Single responsability principle - Princípio da responsabilidade única
Esse princípio declara que uma classe deve ser especializada em um único assunto e possuir apenas uma responsabilidade dentro do software, ou seja, a classe deve ter apenas uma única tarefa.
A ideia aqui é substituir uma classe que faz tudo por classes menores com responsabilidades específicas.
"Uma entidade deve ser responsável por um, e apenas um contexto".

**Não ideal**
```
class Pedido 
{
	public function calcularValorTotal(){/*...*/}
    public function getItens(){/*...*/}
    public function getItemQuantidade(){/*...*/}
    public function adicionarItem($item){/*...*/}
    public function deletarItem($item){/*...*/}

    public function imprimirPedido(){/*...*/}
    public function exibirPedido(){/*...*/}

    public function carregar(){/*...*/}
    public function salvar(){/*...*/}
    public function atualizar(){/*...*/}
    public function deletar(){/*...*/}
}
```

**Ideal**
```
class Pedido
{
    public function calcularValorTotal(){/*...*/}
    public function getItens(){/*...*/}
    public function getItemQuantidade(){/*...*/}
    public function adicionarItem($item){/*...*/}
    public function deletarItem($item){/*...*/}
}

class PedidoRepositorio
{
    public function carregar(){/*...*/}
    public function salvar(){/*...*/}
    public function atualizar(){/*...*/}
    public function deletar(){/*...*/}
}

class PedidoVisualizador
{
    public function imprimirPedido(){/*...*/}
    public function exibirPedido(){/*...*/}
}
```

Dessa forma, 3 responsablidades foram subdivididas em classes menores e específicas para cada função.

### Open-closed principle - Princípio aberto-fechado
Esse princípio indica que entidades devem estar abertas para extensão, mas fechadas para modificação, em outras palavras, devemos ter classes preparadas para receber novos recursos sem a necessidade de alterações.
"Um artefato de software deve ser aberto para extensão, mas fechado para modificação"

**Não ideal**
```
class Relatorio 
{
	public function buscarDados() { /*...*/ }

	public function gerarRelatorio(): void
	{
		$dados = buscarDados();
		/*...*/
	}

	public function gerarRelatorioBarra(): void
	{
		$dados = buscarDados();
		/*...*/
	}

	public function gerarRelatorioPizza(): void
	{
		$dados = buscarDados();
		/*...*/
	}

	public function gerarRelatorioColuna(): void
	{
		$dados = buscarDados();
		/*...*/
	}
}
```

**Ideal**
```
abstract class RelatorioService
{
	public function buscarDados() { /*...*/ }

	public function gerarRelatorio();
}

class RelatorioBarra extends RelatorioService
{
	public function gerarRelatorio()
	{
		buscarDados();
		/*...*/
	}
}

class RelatorioPizza extends RelatorioService
{
	public function gerarRelatorio()
	{
		buscarDados();
		/*...*/
	}
}

class RelatorioColuna extends RelatorioService
{
	public function gerarRelatorio()
	{
		buscarDados();
		/*...*/
	}
}
```

### Liskov substitution principle - Princípio da substituição de Liskov
Esse princípio surgiu através do estudo de Bárbara Liskov sobre subtipos. 
A definição formal para esse item é "Se uma classe A é um subtipo da classe B, então você deve ser capaz de substituir toda ocorrência de B por A mantendo o mesmo comportamento". Em outras palavras podemos dizer que devemos ter a possiblidade de substituir classes filhas por sua classe base sem que altere a corretude do sistema.
"Uma classe derivada deve ser substituível por sua classe base".

**Não ideal**
```
class Animal
{
	// Vocalizar = termo mais abrangente para se referir a qualquer som produzido por um animal para comunicação.
	public function vocalizacao(): void
	{
		echo "Animal vocalizando";
	}
}

class Gato extends Animal
{
	public function vocalizacao(): void
	{
		echo "Miau";
	}
}

class Tartaruga extends Animal
{
	public function vocalizacao(): void
	{
		// Tartaruga não possui vocalização
	}
}
``` 

**Ideal**
```
class Animal
{
	public function comunicacao(): void
	{
		echo "Animal se comunicando";
	}
}

class Gato extends Animal
{
	public function vocalizacao(): void
	{
		echo "Miar";
	}
}

class Tartaruga extends Animal
{
	public function vocalizacao(): void
	{
		echo "Morder, piscar e espirrar água";	// Formas de comunicação entre tartarugas
	}
}
```

### Interface segregation principle - Princípio da segregação de interface
Esse princípio diz que uma classe não deve implementar uma interface que possua um comportamento fora do contexto da classe. Basicamente aqui devemos dar preferência por interfaces mais específicas ao invés de uma interface mais genérica. Dessa forma podemos escolher exatamente quais implementações nossas classes devem possuir e poupa-las de códigos vázios ou inúteis.
"Não devemos fazer o que não é do nosso escopo".

**Não ideal**
```
interface Animal
{
	public function andar();
	public function nadar();
	public function voar();
}

class Cachorro implements Animal
{
	public function andar()
	{
		echo "Andando";
	}

	public function nadar()
	{
		echo "Nadando";
	}

	public function voar()
	{
		// Esse animal não voa
		throw Exception("Ação não realizável para esse animal.");
	}
}
```

**Ideal**
```
interface Andador
{
	public function andar();
}

interface Nadador
{
	public function nadar();
}

interface Voador
{
	public function voar();
}

class Cachorro implements Andador, Nadador
{
	public function andar()
	{
		echo "Andando";
	}

	public function nadar()
	{
		echo "Nadando";
	}
}
```

### Dependency inversion principle - Princípio da inversão de dependências
Esse princípio destaca a importância de depender de abstrações ao invés de implementações. Esse item fundamenta-se na premissa que módulos de software devem depender de interfaces, não de implementações específicas, promovendo desta forma uma estrutura mais simples e flexível. A utilização dessa técnica implica em softwares mais robustos e maleáveis cuja alterações em trechos não impliquem em diversas modificações ou quebras.
Segundo o uncle Bob, esse princípio diz que: 
1. Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender da abstração.
2. Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações.
"Dependa de abstrações e não de implementações".

**Não ideal**
```
class Lampada
{
	public function ligar(): void
	{
		echo "Lâmpada ligada";
	}

	public function desligar(): void
	{
		echo "Lâmpada desligada";
	}
}

class Interruptor
{
	private Lampada $lampada;

	public __construct()
	{
		$this->lampada = new Lampada();
	}

	public function ligar(): void
	{
		$this->lampada->ligar();
	}

	public function desligar(): void
	{
		$this->lampada->desligar();
	}
}
```

**Ideal**
```
public interface DispositivoControlavel
{
	public function ligar(): void;
	public function desligar(): void;
}

public class Interruptor
{
	private DispositivoControlavel $dispositivo;

	public __construct(DispositivoControlavel $dispositivo)
	{
		$this->dispositivo = $dispositivo;
	}

	public function ligar(): void
	{
		$this->dispositivo->ligar();
	}

	public function desligar(): void
	{
		$this->dispositivo->desligar();
	}
}
```