<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aula Pratica - Salários</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

</head>
<body>
    
    <?php

    if(isset($_POST['salvar'])){
        $nome = $_POST['nome'];
        $salario_bruto = $_POST['salario-bruto'];
        $arquivo = 'arquivos/arquivo.txt';
        $tamanhoArquivo = filesize($arquivo);

        function desconto_inss($salario_bruto){
            if($salario_bruto <= 1045){
                $aliquota = $salario_bruto * 0.075;
            }
        
            elseif($salario_bruto > 1045 && $salario_bruto <= 2089.60){
                $aliquota = $salario_bruto * 0.09;
            }
        
            elseif($salario_bruto > 1045 && $salario_bruto <= 2089.60){
                $aliquota = $salario_bruto * 0.12;
            }
        
            else{
                $aliquota = $salario_bruto * 0.14;
            }
        
            return $salario_bruto - $aliquota;
        }
        
        $salario = desconto_inss($salario_bruto);
        
        function desconto_irpf($salario){
            if($salario <= 1903.98){
                $aliquota = 0.0;
                $deducao = 0.0;
            }
        
            elseif($salario > 1903.98 && $salario <= 2826.65) {
                $aliquota = $salario * 0.075;
                $deducao = 142.80;  
            }
        
            elseif($salario > 2826.65 && $salario <= 3751.05){
                $aliquota = $salario * 0.15;
                $deducao = 354.80;
            }
        
            elseif($salario > 3751.05 && $salario <= 4664.68){
                $aliquota = $salario * 0.225;
                $deducao = 636.13;
            }
        
            elseif($salario > 4664.68){
                $aliquota = $salario * 0.275;
                $deducao = 869.36;
            }
        
            return $salario - $aliquota + $deducao;
        }
        
        $salario_final = desconto_irpf($salario);
        $arquivoAberto = fopen($arquivo, 'arw');
            fwrite($arquivoAberto, "Nome: ".$nome."  |");
            fwrite($arquivoAberto, "Salário Bruto: R$".$salario_bruto."  |");
            fwrite($arquivoAberto, "Salário Líquido: R$".$salario_final."  |");
        fclose($arquivoAberto);
    }

    ?>

    <div class="container">
        <div class="row">
            <div class="col mt-5" name="formulario">
                <form method="POST" enctype="multipart/form-data">
                    <div class="form-group">
                      <label for="nome">Nome do Usuário</label>
                        <input type="text" class="form-control" name="nome" required>
                      <small id="nomeSubTop" class="form-text text-muted">Sobrenome não será necessário!</small>
                    </div>
                    <div class="form-group">
                      <label for="salarioBruto">Salário Bruto</label>
                        <input type="number" class="form-control" name="salario-bruto" required>
                      <small id="salarioSubtop" class="form-text text-muted">Seus dados guardados com segurança!</small>
                    </div>
                    
                    <button type="submit" class="btn btn-primary" value="salvar"name="salvar">Salvar Dados</button>
                  </form>
            </div>
        </div>
    </div>

    <div class="container">
        <div class="row">
            <div class="col mt-5">
                <div class="list-group">
                    <button type="button" class="list-group-item active">INFORMAÇÕES</button>
                    <button type="button" class="list-group-item list-group-item-action"><?php
                        if(isset($_POST['salvar'])){
                            $dados = file_get_contents("arquivos/arquivo.txt");
                            $dados_separados = explode("|", $dados);
                            foreach($dados_separados as $key => $value){
                                if(!(($key+1) %3 == 0)){
                                    echo($value);
                                }else{
                                    echo($value."<br>");
                                }
                                
                            }

                        }
                    
                    ?></button>
                    
                </div>
            </div>
        </div>
      </div>

</body>
</html>
