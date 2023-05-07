# Validador-cpf
<?php
function validaCPF($cpf) {

    // Extrai apenas os números do CPF
    $cpf = preg_replace('/[^0-9]/', '', $cpf);

    // Verifica se passou apenas número repetidos
    if (preg_match('/(\d)\1{10}/', $cpf)) {
        return false;
    }
    
    // Verifica se o tamanho do CPF é válido
    if (strlen($cpf) != 11) {
        return false;
    }

    // Calcula e verifica o primeiro dígito verificador
    $soma = 0;
    for ($i = 0; $i < 9; $i++) {
        $soma += (int) substr($cpf, $i, 1) * (10 - $i);
    }
    $resto = $soma % 11;
    $dv1 = $resto < 2 ? 0 : 11 - $resto;

    // Calcula e verifica o segundo dígito verificador
    $soma = 0;
    for ($i = 0; $i < 9; $i++) {
        $soma += (int) substr($cpf, $i, 1) * (11 - $i);
    }
    $soma += $dv1 * 2;
    $resto = $soma % 11;
    $dv2 = $resto < 2 ? 0 : 11 - $resto;

    // Retorna true se os dígitos verificadores baterem
    return $cpf[9] == $dv1 && $cpf[10] == $dv2;

}

// Exemplo de uso
if (validaCPF('123.456.789-09')) {
    echo 'CPF válido!';
} else {
    echo 'CPF inválido!';
}
