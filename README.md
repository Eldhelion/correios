# Correios

----------------------
Fork do cpbdigital/correios.
Package para consulta de serviços diretamente no site dos correios, sem usar apis de terceiros.

Baseado nos seguintes repositório:
- https://github.com/cagartner/correios-consulta

Requerimentos:
- Soap Client

Para linux: `sudo apt-get install php-soap`

Consultas disponíveis:
- CEP
- Frete
- Rastreio

### Instalação

In the `require` key of `composer.json` file add the following

    "cpbdigital/correios": "1.0.0"

Run the Composer update comand

    $ composer update


### Utilização

#### CEP:

Passar apenas o valor do CEP, pode ser formatado, somente números e como string.

```php
<?php
    echo Correios::cep('89062086');
    
    /*
        Retorno:
        Array
        (
            [cliente] => 
            [logradouro] => Rua Lindolfo Kuhnen
            [bairro] => Itoupava Central
            [cep] => 89062086
            [cidade] => Blumenau
            [uf] => SC
        )
    */

```

#### Rastrear

Passar o código de rastreio informado pelos Correios

```php
<?php
    echo Correios::rastrear('PI464134876BR');
    
    /*
        Retorno:
        Array
        (
            [0] => Array
                (
                    [data] => 08/06/2015 14:47
                    [local] => CDD CARAGUATATUBA - Caraguatatuba/SP
                    [status] => Entrega Efetuada
                )

            [1] => Array
                (
                    [data] => 08/06/2015 07:59
                    [local] => Caraguatatuba/SP
                    [status] => Saiu para entrega ao destinat�rio
                )

            [2] => Array
                (
                    [data] => 03/06/2015 11:48
                    [local] => CTE SAO JOSE DOS CAMPOS - Sao Jose Dos Campos/SP
                    [status] => Encaminhado
                    [encaminhado] => Em tr�nsito para CDD CARAGUATATUBA - Caraguatatuba/SP
                )

            [3] => Array
                (
                    [data] => 02/06/2015 10:00
                    [local] => AGF DOUTOR JOAO MENDES - Sao Paulo/SP
                    [status] => Encaminhado
                    [encaminhado] => Em tr�nsito para CTE VILA MARIA - Sao Paulo/SP
                )

            [4] => Array
                (
                    [data] => 01/06/2015 14:56
                    [local] => AGF DOUTOR JOAO MENDES - Sao Paulo/SP
                    [status] => Postado
                )

        )
    */

```

#### C�lculo de Frete:

```php
<?php
    $dados = [
        'tipo'              => 'sedex', // Separar op��es por v�rgula (,) caso queira consultar mais de um (1) servi�o. > Op��es: `sedex`, `sedex_a_cobrar`, `sedex_10`, `sedex_hoje`, `pac`, 'pac_contrato', 'sedex_contrato' , 'esedex'
        'formato'           => 'caixa', // op��es: `caixa`, `rolo`, `envelope`
        'cep_destino'       => '89062086', // Obrigat�rio
        'cep_origem'        => '89062080', // Obrigatorio
        //'empresa'         => '', // Código da empresa junto aos correios, n�o obrigat�rio.
        //'senha'           => '', // Senha da empresa junto aos correios, n�o obrigat�rio.
        'peso'              => '1', // Peso em kilos
        'comprimento'       => '16', // Em cent�metros
        'altura'            => '11', // Em cent�metros
        'largura'           => '11', // Em cent�metros
        'diametro'          => '0', // Em cent�metros, no caso de rolo
        // 'mao_propria'       => '1', // N�o obrigat�rios
        // 'valor_declarado'   => '1', // N�o obrigat�rios
        // 'aviso_recebimento' => '1', // N�o obrigat�rios
    ];

    echo Correios::frete($dados);
    
    /*
        Retorno para uma �nica consulta:
        Array
        (
            [codigo] => 40010
            [valor] => 14.9
            [prazo] => 1
            [mao_propria] => 0
            [aviso_recebimento] => 0
            [valor_declarado] => 0
            [entrega_domiciliar] => 1
            [entrega_sabado] => 1
            [erro] => Array
                (
                    [codigo] => 0
                    [mensagem] => 
                )
        )
    */

    /*
        Retorno para v�rias consultas:
        Array
        (
            0 => Array
            (
                [codigo] => 4510
                [valor] => 14.9
                [prazo] => 1
                [mao_propria] => 0
                [aviso_recebimento] => 0
                [valor_declarado] => 0
                [entrega_domiciliar] => 1
                [entrega_sabado] => 1
                [erro] => Array
                    (
                        [codigo] => 0
                        [mensagem] => 
                    )
            ),
            1 => Array
            (
                [codigo] => 4014
                [valor] => 14.9
                [prazo] => 1
                [mao_propria] => 0
                [aviso_recebimento] => 0
                [valor_declarado] => 0
                [entrega_domiciliar] => 1
                [entrega_sabado] => 1
                [erro] => Array
                    (
                        [codigo] => 0
                        [mensagem] => 
                    )
            )
        )
    */

```
