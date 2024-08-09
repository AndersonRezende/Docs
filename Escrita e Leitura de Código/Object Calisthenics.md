# Object Calisthenics - calistenia de objetos

Foi introduzida por Jeff Bay no livro chamado Thought Works Anthology.

## Objetivo
O principal objetivo desse tópico é aplicar alguns princípios do SOLID. Basicamente são um conjunto de boas práticas e regras de programação para aumentar a qualidade do código.
A aplicação dessas regras costuma aumentar a qualidade do código, concentrando na manutenibilidade, legibilidade, testabilidade e compreensão.

## Regras básicas do Object Calisthenics
<ol>
	<li>Somente um nível de indentação por método</li>
	<li>Não utilizar a palavra reservada "ELSE"</li>
	<li>Encapsular os tipos primitivos e Strings</li>
    <li>Use coleções de primeira classe</li>
	<li>Utilize apenas um ponto por linha</li>
	<li>Não abrevie</li>
	<li>Mantenha todas as entidades pequenas</li>
	<li>Não use nenhuma classe com mais de duas variáveis de instância</li>
	<li>Não use getters/setters/properties</li>
</ol>

## Na prática

### 1. Somente um nível de indentação
Essa regra diz para criar métodos com no máximo um nível de indentação.
Tente manter o seu código o mais simples e compreensível possível. Ao escrever métodos menores, você pode notar mais oportunidades de reutilizar seu código e eliminar código duplicado.
"Dividir para conquistar".

**Não ideal**
```
class Customer
{
    public function getPromoCode(string $promoName)
    {
        // 1.
        if ($this->promoCode) {
            // 2.
            if (false === $this->promoCodeExpired()) {
                // 3.
                if ($this->promoName == $promoname) {
                    return $this->promoCode;
                } else {
                    throw new Exception('Promoção não existe mais');
                }
            } else {
                throw new Exception('Promoção Expirada');
            }      
        } else {
            throw new Exception('Cliente sem código de promoção');
        }
    }
}
```
**Ideal**
```
class Customer
{
    public function getPromoCode(string $promoName)
    {
        if ($this->promoCode) {
            return $this->getValidPromoCode($promoName);
        } else {
            throw new Exception('Cliente sem código de promoção');
        }
    }

    protected function getValidPromoCode(string $promoName)
    {
        if (false === $this->promoCodeExpired()) {
            return $this->getPromoExists($promoName);
        } else {
            throw new Exception('Promoção Expirada');
        }    
    }

    protected function getPromoExists(string $promoName)
    {
        if ($this->promoName == $promoName) {
            return $this->promoCode;
        } else {
            throw new Exception('Promoção não existe mais');
        }
    }
}

```

### 2. Não utilizar a palavra reservada "ELSE"
A ideia aqui é evitar ao máximo a utilização da palavra "else" através da checagem do fluxo incorreto.
Essa palavra está associada a um caminho diferente do fluxo princinpal, logo se precisamos validar se algo está no "trilho" correto, por que não checar se estar incorreto antes e já encerrar o fluxo?
O nome dessa técnica é **early Return (retorno antecipado)** e **fail first (falhe primeiro)**.
Outra possível solução para essa técnica é a utilização de **Polimorfismo**.
"Poupe esforços".

**Não ideal**
```
public function dividir($dividendo, $divisor) {
    $resultado = 0;
    if($divisor != 0) {
        $resultado = $dividendo / $divisor;
    } else {
        throw new Exception("Division by zero");
    }
    return $resultado;
}
```
**Ideal**
```
public function dividir($dividendo, $divisor) {
    if($divisor == 0) {
        throw new Exception("Division by zero");
    }
    return $dividendo / $divisor;
}
```

### 3. Encapsular os tipos primitivos e Strings
Essa regra consiste em encapsular todos os tipos primitivos como objetos.
Se um tipo primitivo tem um comportamento, então essa variável deveria se transformar em um objeto.
"Devemos trabalhar com o que é nossa responsabilidade".

**Não ideal**
```
public class Pessoa
{
    public string CPF { get; set; }
}
```
**Ideal**
```
public class Pessoa
{
    public CPF CPF { get; set; }
}

public class CPF
{
    public string Numero { get; set; }
    public string NumeroFormatado => FormataNumero();
    public CPF (string numero)
    {
        if (!EstaValido(numero))
            throw new CPFInvalidoException(numero);
        this.Numero = numero;
    }
    private void EstaValido(numero)
    {
        //Codigo..
    }
    private String FormataNumero()
    {
        //Codigo
    }
}
```

### 4. Use coleções de primeira classe
Qualquer classe que contenha uma coleção não deve conter outras variáveis de membro. Se você tiver um conjunto de elementos e quiser manipulá-los, crie uma classe dedicada para essa coleção. Assim comportamentos relacionados a coleção serão implementados por sua própria classe.
"Não assuma responsabilidade alheia".

**Não ideal**
```
class Curso
{
    private string $turma;
    private string $turno;
    /*...*/
    private $videos = [];

    public function add(string $video) {/***/}
    public function remove(int $videoIndex) {/***/}
    public function count(): int {/***/}
}
```

**Ideal**
```
class Videos
{
  private $videos = [];

  public function add(string $video) {/***/}
  public function remove(int $videoIndex) {/***/}
  public function count(): int {/***/}
}

class Curso
{
    private string $turma;
    private string $turno;
    /*...*/
    private Videos $videos;

    /*...*/
}
```

### 5. Utilize apenas um ponto por linha
Usar apenas um ponto por linha tornará seu código mais legível ao eliminar chamadas de método em cadeia.
"Apenas fale com seus amigos".

**Não ideal**
```
class Carro
{
  public function __construct(Fabricante $fabricante)
  {
    $this->fabricante = $fabricante;
  }
}

class Fabricante
{
  public function __construct(string $nome)
  {
    $this->nome = $nome;
  }
}

$fabricante = $carro->fabricante->nome;
```

**Ideal**
```
class Carro
{
  public function __construct(Fabricante $fabricante)
  {
    $this->fabricante = $fabricante;
  }

  public function nomeFabricante()
  {
    return $this->fabricante->nome;
  }
}

$nomeFabricante = $carro->nomeFabricante();
```

### 6. Não abrevie
Devemos nomear os itens de forma clara e precisa, facilitando o máximo o seu entendimento e poupando que o leitor necessite navegar pelo código para entender o contexto do item e sua funcionalidade.
"Não estamos escrevendo mensagens criptografadas, seja claro".

**Não ideal**
```
class C extends Automovel {
    public function acl(): void {
        //
    }
}
$c = new C();
$c->acl();
```

**Ideal**
```
class Carro extends Automovel {
    public function acelerar(): void {
        //
    }
}

$carro = new Carro();
$carro->acelerar();
```

### 7. Mantenha todas as entidades pequenas
Manter entidades pequenas facilita a leitura e compreensão da mesma, sem falar que, provavelmente, entidades muito grandes possam implicar na quebra do princípio da responsabilidade única (S do SOLID). 
Embora a recomendação seja que classes não passem de 50 linhas e pacotes não passem de 10 arquivos, cabe a utilização do bom senso e entender o que realmente é válido para a situação em questão.
"Um artigo preciso é mais fácil de ler do que uma extensa monografia".

**Não ideal**
```
class Usuario {
    /*...*/

    public function getNome(): string { /*...*/ }

    public function setNome(string $nome): void { /*...*/ }

    /*...*/

    public function salvarUsuario(): void { /*...*/ }

    /*...*/

    public function enviarEmail(): void { 
        // 100 linhas
    }

    // 500 linhas
}
```

**Ideal**
```
Usuario.php ->  30 linhas
UsuarioRepositorio.php  -> 50 linhas
UsuarioEmail.php    ->  25 linhas
```

### 8. Não use nenhuma classe com mais de duas variáveis de instância
Essa regra, semelhante a outra, serve mais como um guia para alertar sobre uma possível quebra do S do SOLID do que como um requisito para ser seguido ao pé da letra. Ela diz que não devemos ter mais do que duas variáveis de instância numa mesma classe. Ela serve como guia para checar a coesão (responsabilidade única) de uma classe.
"Não assuma todas as funções, divida as tarefas para cada responsável".

**Não ideal**
```
class Estudante
{
  public $nome;
  public $email;
  public $telefone;
  public $endereco;
  public $cidade;
  public $pais;
  public $cursos;
  public $notas;
  ...
}

$estudante = new Estudante(
  "Fulano de tal",
  "fulanodetal@gmail.com",
  "+5587900000000",
  "...",
  "...",
  "...",
  "...",
  "..."
);
```

**Ideal**
```
class Estudante
{
  public $pessoa;
  public $endereco;
  public $cursos;
}

$pessoa = new Person(
  "Fulano de tal",
  "fulanodetal@gmail.com",
  "+5587900000000",
);
$endereco = new Endereco("Lorem Ipsum dolor", "..", "..");
$cursos = new Curso([...], [...]);
$estudante = new Estudante($pessoa, $endereco, $cursos);
```

### 9. Não use getters/setters/properties
Embora um pouco estranha, essa regra indica que não devemos utilizar getters e setters nas nossas classes. A ideia aqui é utilizar o conceito da frase "Tell, don´t ask (diga, não pergunte)", não devemos nos preocupar com a lógica "alheia", mas sim em informar o que precisamos que ocorra.
A lógica deve ser realizada pela classe específica, externos devem apenas informar o que eles querem que seja feito.
"Entenda a necessidade ao invés de repetir costumes".

**Não ideal**
```
class Pontuacao {
    private int pontos;

    public function setPontos(int pontos): void {
        $this->pontos = pontos;
    }

    public function getPontos(): int {
        return $this->pontos;
    }
}

$pontos->setPontos($pontos->getPontos + 1);
```

**Ideal**
```
class Pontuacao {
    private int pontos;

    public function adicionarPontos(int pontos): void {
        $this->pontos += pontos;
    }

    public function pontos(): int {
        return $this->pontos;
    }
}

$pontos->adicionarPontos(10);
```