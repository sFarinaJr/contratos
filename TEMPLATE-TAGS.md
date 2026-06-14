# Como editar os templates `clinica.docx` e `face.docx`

Estes dois arquivos `.docx` são os **modelos** do contrato. Você pode abrir no Word/Google Docs
e **editar livremente todo o texto** (cláusulas, redação, formatação, ordem) — **desde que não
altere as tags entre chaves duplas** `{{ ... }}`. São elas que o sistema preenche automaticamente
com os dados digitados no site.

## Regra de ouro
- **Pode** mudar qualquer texto, negrito, espaçamento, títulos, adicionar ou remover cláusulas.
- **Não** apague, traduza nem altere o que está dentro de `{{ }}`.
- Toda tag de abertura precisa da sua tag de fechamento (ver loops abaixo).
- Se quiser mover um campo de lugar, **recorte e cole a tag inteira** `{{nome}}` (com as chaves).

## Campos simples (preenchidos uma vez)
| Tag | O que vira |
|-----|------------|
| `{{inicioContrato}}` | Data de início por extenso (ex.: 14 de junho de 2026) |
| `{{vencimento}}` | Dia de vencimento do aluguel |
| `{{valorAluguel}}` | Valor mensal (ex.: 2.500,00) |
| `{{prazoContrato}}` | Permanência mínima (ex.: 12 (doze) meses) |

## Locatários (repete por inquilino)
Tudo que estiver **entre** `{{#locatarios}}` e `{{/locatarios}}` é repetido uma vez para cada
locatário informado no site. Dentro do bloco, use os campos da pessoa:

```
{{#locatarios}}
LOCATÁRIO {{numero}}: {{nome}}, ... RG ou CRO {{rg}}, CPF {{cpf}}, ... {{endereco}}, {{cep}},
{{cidade}}/{{estado}}, {{email}}, {{whats}}.
{{/locatarios}}
```
Campos disponíveis: `{{numero}}` `{{nome}}` `{{rg}}` `{{cpf}}` `{{endereco}}` `{{cep}}`
`{{cidade}}` `{{estado}}` `{{email}}` `{{whats}}`.

> A **Solicitação de Integração ao Corpo Clínico** também está dentro de um bloco
> `{{#locatarios}} … {{/locatarios}}`, por isso sai **uma por inquilino**, já preenchida.

## Fiadores (repete por fiador)
Entre `{{#fiadores}}` e `{{/fiadores}}`. Mesmos campos da pessoa, mais:
- `{{estadoCivil}}` — estado civil do fiador.
- `{{anuencia}}` — frase automática da anuência do cônjuge (vazia se não houver).

### Cônjuge anuente (condicional)
Dentro do bloco de fiadores, o que estiver entre `{{#temConjuge}}` e `{{/temConjuge}}` **só
aparece se o fiador for casado/união estável e tiver cônjuge informado**:
```
{{#temConjuge}}
{{conjugeNome}} – CÔNJUGE ANUENTE DO FIADOR {{numero}}
{{/temConjuge}}
```

## Resumo dos blocos (sempre em par)
| Abre | Fecha | Repete/condiciona |
|------|-------|-------------------|
| `{{#locatarios}}` | `{{/locatarios}}` | um por locatário |
| `{{#fiadores}}` | `{{/fiadores}}` | um por fiador |
| `{{#temConjuge}}` | `{{/temConjuge}}` | só se houver cônjuge anuente |

## Se algo quebrar
Se o site mostrar erro de preenchimento, normalmente é uma tag incompleta (faltou `}}`) ou um
bloco sem fechamento. Volte ao `.docx`, confira os pares acima e salve novamente.
Há cópias de segurança `.docx.bak*` na pasta caso precise restaurar.
