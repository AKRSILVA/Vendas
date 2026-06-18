-- 1° Entrar no schema pedido_venda

SET search_path TO pedido_venda;

---------------------------------------------------------------------

-- 2° Criação das tabelas e insert da planilha

CREATE TABLE IF NOT EXISTS pedido_venda.produto (
    cd_produto SERIAL PRIMARY KEY,
    barras_produto BIGINT NOT NULL,
    nome_produto TEXT NOT NULL,
    vl_unit_produto NUMERIC(15, 2) NOT NULL,
    ativo_produto BOOLEAN NOT NULL DEFAULT TRUE,
    tipo_produto TEXT NOT NULL
);

-- Confirmar criação da tabela produto

select * from produto;

CREATE TABLE IF NOT EXISTS pedido_venda.servico (
    cd_servico SERIAL PRIMARY KEY,
    nome_servico TEXT NOT NULL,
    vl_unit_servico NUMERIC(15, 2) NOT NULL,
    ativo_servico BOOLEAN NOT NULL DEFAULT TRUE,
    tipo_servico TEXT
);

-- Confirmar criação da tabela servico

select * from servico;

----------- Importar dados da tabela pelo produto Phyton no arquivo "importa_produto.py"
----------- Importar dados da tabela servico pelo Phyton no arquivoimporta_servico_py

--Confirmação da importação dos dados
select * from servico;
select * from produto;


CREATE TABLE escolaridade (
    cd_esc SERIAL PRIMARY KEY,
    descricao_esc VARCHAR(50) NOT NULL UNIQUE
);

-- Confirmar criação da tabela escolaridade

select * from escolaridade


CREATE TABLE tipo_telefone (
    cd_tipo_tel SERIAL PRIMARY KEY,
    desc_tipo_tel VARCHAR(20) NOT NULL UNIQUE
);

-- Confirmar criação da tabela tipo_telefone 

select * from tipo_telefone


CREATE TABLE tipo_endereco (
    cd_tipo_end SERIAL PRIMARY KEY,
    descricao_tipo_end VARCHAR(20) NOT NULL UNIQUE
);

-- Confirmar criação da tabela tipo_endereco 

select * from tipo_endereco

CREATE TABLE cliente (
    cd_cliente SERIAL PRIMARY KEY,
    nome_cliente VARCHAR(100) NOT NULL,
    data_nasc_cliente DATE NOT NULL,
    escolaridade_fk INTEGER NOT NULL REFERENCES escolaridade(cd_esc),
    ativo_cliente BOOLEAN NOT NULL
);

-- Confirmar criação da tabela cliente 

select * from cliente


CREATE TABLE telefone (
    cd_tel SERIAL PRIMARY KEY,
    cliente_id INTEGER NOT NULL REFERENCES cliente(cd_cliente),
    numero_tel VARCHAR(20) NOT NULL,
    tipo_telefone_fk INTEGER NOT NULL REFERENCES tipo_telefone(cd_tipo_tel),
    dt_atualizacao_tel TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Confirmar criação da tabela telefone 

select * from telefone

CREATE TABLE endereco (
    cd_end SERIAL PRIMARY KEY,
    cliente_fk INTEGER NOT NULL REFERENCES cliente(cd_cliente),
    tipo_endereco_id INTEGER NOT NULL REFERENCES tipo_endereco(cd_tipo_end),
    cidade_end VARCHAR(50) NOT NULL,
    endereco_end VARCHAR(100) NOT NULL,
    bairro_end VARCHAR(50) NOT NULL,
    numero_end VARCHAR(10) NOT NULL,
    complemento_end VARCHAR(50)
);

-- Confirmar criação da telefone 

select * from telefone


-- 3° Insert no banco das tabelas escolaridade, tipo_telefone, tipo_endereco

-- Insert nas tabelas

INSERT INTO escolaridade (descricao_esc) VALUES 
  ('Ensino Fundamental'), 
  ('Ensino Medio'), 
  ('Ensino Superior'), 
  ('Mestrado'), 
  ('Doutorado');


INSERT INTO tipo_telefone (desc_tipo_tel) VALUES 
  ('Celular'), 
  ('Fixo');
  
  
INSERT INTO tipo_endereco (descricao_tipo_end) VALUES 
  ('Residencial'), 
  ('Comercial'), 
  ('Entrega');
  
  
-- 4° Inserir no banco das tabelas cliente, telefone, endereco

INSERT INTO cliente (nome_cliente, data_nasc_cliente, escolaridade_fk, ativo_cliente) VALUES
('Ana Silva','2000-01-18', 1, TRUE),
('Joao Silva','2001-02-18', 2, TRUE),
('Maria Teresa','2002-03-18', 3, TRUE),
('Daniel Vasconselos','1999-04-18', 4, TRUE),
('Josue Passos','1989-05-18', 5, TRUE),
('Rodrigo Farias','1991-06-18', 1, TRUE),
('Roberto Farias','2000-07-18', 2, TRUE),
('Larissa Costa','2001-09-18', 3, TRUE),
('Tobias Oliveira','1990-10-15', 4, TRUE),
('Maiara Macedo','1999-11-15', 5, TRUE);


INSERT INTO telefone (cliente_id, numero_tel, tipo_telefone_fk) VALUES

(1, '47991230001', 1), (1, '47337300001', 2),
(2, '47991230002', 1), (2, '47337300002', 2),
(3, '47991230003', 1), (3, '47337300003', 2),
(4, '47991230004', 1), (4, '47337300004', 2),
(5, '47991230005', 1), (5, '47337300005', 2),
(6, '47991230006', 1), (6, '47337300006', 2),
(7, '47991230007', 1), (7, '47337300007', 2),
(8, '47991230008', 1), (8, '47337300008', 2),
(9, '47991230009', 1), (9, '47337300009', 2),
(10, '47991230010', 1), (10, '47337300010', 2);

INSERT INTO endereco (cliente_fk, tipo_endereco_id, cidade_end, endereco_end, bairro_end, numero_end, complemento_end) VALUES
(1, 1, 'Florianópolis', 'Rua das Flores', 'Centro', '100', 'Apto 101'),
(1, 3, 'Florianópolis', 'Av. Beira Mar', 'Estreito', '200', 'Casa'),
(2, 1, 'Joinville', 'Rua XV de Novembro', 'Glória', '120', ''),
(2, 3, 'Joinville', 'Av. Santos Dumont', 'Bom Retiro', '500', ''),
(3, 1, 'Blumenau', 'Rua São Paulo', 'Victor Konder', '345', 'A'),
(3, 3, 'Blumenau', 'Rua Itajaí', 'Velha', '132', ''),
(4, 1, 'Chapecó', 'Rua General Osório', 'Centro', '78', ''),
(4, 3, 'Chapecó', 'Av. Getúlio Vargas', 'Efapi', '90', 'Fundos'),
(5, 1, 'Criciúma', 'Rua Henrique Lage', 'Michel', '234', ''),
(5, 3, 'Criciúma', 'Rua da Paz', 'Santa Bárbara', '654', ''),
(6, 1, 'Lages', 'Rua Frei Rogério', 'Centro', '88', ''),
(6, 3, 'Lages', 'Rua Correia Pinto', 'Coral', '99', ''),
(7, 1, 'São José', 'Rua Domingos André Zanini', 'Campinas', '45', 'Sala 2'),
(7, 3, 'São José', 'Rua Koesa', 'Kobrasol', '300', ''),
(8, 1, 'Itajaí', 'Av. Sete de Setembro', 'Centro', '78', ''),
(8, 3, 'Itajaí', 'Rua Brusque', 'São Vicente', '999', ''),
(9, 1, 'Palhoça', 'Rua Nazareno Coelho', 'Centro', '120', ''),
(9, 3, 'Palhoça', 'Av. Barão do Rio Branco', 'Pagani', '45', ''),
(10, 1, 'Balneário Camboriú', 'Av. Brasil', 'Pioneiros', '200', ''),
(10, 3, 'Balneário Camboriú', 'Rua 3000', 'Centro', '350', '');

----------------Pedido----------------------


-- 5° Criar tabelas

CREATE TABLE pedido (
    cd_pedido SERIAL PRIMARY KEY,
    cliente_fk INTEGER NOT NULL REFERENCES cliente(cd_cliente),
    situacao VARCHAR(20) NOT NULL CHECK (situacao IN ('Em Andamento', 'Pago'))
);

-- -- Confirmar criação da tabela pedido 

select * from pedido

CREATE TABLE item_pedido (
    cd_item SERIAL PRIMARY KEY,
    pedido_fk INTEGER NOT NULL REFERENCES pedido(cd_pedido),
    tipo_item VARCHAR(10) NOT NULL CHECK (tipo_item IN ('Produto', 'Servico')),
    id_item INTEGER NOT NULL,
    quantidade INTEGER NOT NULL CHECK (quantidade > 0),
    perc_acrescimo NUMERIC(5,2) DEFAULT 0,
    vl_acrescimo NUMERIC(10,2) DEFAULT 0,
    perc_desconto NUMERIC(5,2) DEFAULT 0,
    vl_desconto NUMERIC(10,2) DEFAULT 0,
    vl_unitario NUMERIC(10,2) NOT NULL CHECK (vl_unitario > 0),
    CONSTRAINT uk_item_unico_por_pedido UNIQUE (pedido_fk, tipo_item, id_item)
);

-- Confirmar criação da tabela item_pedido

select * from item_pedido

CREATE TABLE forma_pagamento (
    cd_fp SERIAL PRIMARY KEY,
    descricao VARCHAR(20) NOT NULL UNIQUE
);

-- Confirmar criação da tabela forma_pagamento

select * from forma_pagamento 


CREATE TABLE pagamento_pedido (
    pedido_fk INTEGER NOT NULL REFERENCES pedido(cd_pedido),
    forma_pagamento_fk INTEGER NOT NULL REFERENCES forma_pagamento(cd_fp),
    valor_pago NUMERIC(10,2) NOT NULL CHECK (valor_pago > 0),
    PRIMARY KEY (pedido_fk, forma_pagamento_fk)
);

-- Confirmar criação da pagamento_pedido

select * from pagamento_pedido


-- 6° Insert na tabela Carga inicial

INSERT INTO forma_pagamento (descricao) VALUES
('Dinheiro'), ('Cartão de Credito'), ('Cartao de Debito'), ('Boleto'), ('Pix');

-- 7° Criação das Views 

--------view_pedidos

CREATE OR REPLACE VIEW view_pedidos AS
SELECT
  p.cd_pedido,
  p.situacao,
  c.nome_cliente,
  c.data_nasc_cliente,
  e.descricao_esc AS escolaridade,
  t.numero_tel,
  tt.desc_tipo_tel,
  STRING_AGG(
    te.descricao_tipo_end || ': ' || e2.cidade_end || ', ' || e2.endereco_end || ', ' || e2.bairro_end || ', ' || e2.numero_end,
    ' | '
  ) AS enderecos,

  COALESCE(itens.subtotal, 0) AS subtotal,
  COALESCE(itens.total_acrescimo, 0) AS total_acrescimo,
  COALESCE(itens.total_desconto, 0) AS total_desconto,
  COALESCE(itens.total_liquido, 0) AS total_liquido,
  COALESCE(pag.total_pago, 0) AS total_pago

FROM pedido p
JOIN cliente c ON c.cd_cliente = p.cliente_fk
JOIN escolaridade e ON e.cd_esc = c.escolaridade_fk


LEFT JOIN (
    SELECT DISTINCT ON (cliente_id) *
    FROM telefone
    ORDER BY cliente_id, dt_atualizacao_tel DESC
) t ON t.cliente_id = c.cd_cliente

LEFT JOIN tipo_telefone tt ON tt.cd_tipo_tel = t.tipo_telefone_fk

LEFT JOIN endereco e2 ON e2.cliente_fk = c.cd_cliente
LEFT JOIN tipo_endereco te ON te.cd_tipo_end = e2.tipo_endereco_id

LEFT JOIN (
    SELECT 
        pedido_fk,
        SUM(vl_unitario * quantidade) AS subtotal,
        SUM(vl_acrescimo) AS total_acrescimo,
        SUM(vl_desconto) AS total_desconto,
        SUM((vl_unitario * quantidade) + vl_acrescimo - vl_desconto) AS total_liquido
    FROM item_pedido
    GROUP BY pedido_fk
) itens ON itens.pedido_fk = p.cd_pedido

LEFT JOIN (
    SELECT pedido_fk, SUM(valor_pago) AS total_pago
    FROM pagamento_pedido
    GROUP BY pedido_fk
) pag ON pag.pedido_fk = p.cd_pedido

GROUP BY
  p.cd_pedido,
  p.situacao,
  c.nome_cliente,
  c.data_nasc_cliente,
  e.descricao_esc,
  t.numero_tel,
  tt.desc_tipo_tel,
  itens.subtotal,
  itens.total_acrescimo,
  itens.total_desconto,
  itens.total_liquido,
  pag.total_pago;



-------- view_pedidos_pagos

CREATE OR REPLACE VIEW view_pedidos_pagos AS
SELECT
  p.cd_pedido,
  p.situacao,
  fp.descricao AS forma_pagamento,
  pp.valor_pago,
  SUM(pp.valor_pago) OVER (PARTITION BY p.cd_pedido) AS total_pago,
  SUM((ip.vl_unitario * ip.quantidade) + ip.vl_acrescimo - ip.vl_desconto) AS total_liquido
FROM pedido p
JOIN pagamento_pedido pp ON pp.pedido_fk = p.cd_pedido
JOIN forma_pagamento fp ON fp.cd_fp = pp.forma_pagamento_fk
JOIN item_pedido ip ON ip.pedido_fk = p.cd_pedido
WHERE p.situacao = 'Pago'
GROUP BY p.cd_pedido, p.situacao, fp.descricao, pp.valor_pago;


--------  view_mais_vendidos

CREATE VIEW view_mais_vendidos AS
SELECT
  tipo_item,
  id_item,
  CASE WHEN tipo_item = 'Produto' THEN pr.nome_produto ELSE sv.nome_servico END AS nome,
  CASE WHEN tipo_item = 'Produto' THEN pr.barras_produto ELSE NULL END AS cod_barras,
  CASE WHEN tipo_item = 'Produto' THEN pr.vl_unit_produto ELSE sv.vl_unit_servico END AS valor_unitario,
  COUNT(*) AS qtd_ocorrencias,
  SUM(ip.quantidade) AS soma_quantidade
FROM item_pedido ip
LEFT JOIN produto pr ON pr.cd_produto = ip.id_item AND ip.tipo_item = 'Produto'
LEFT JOIN servico sv ON sv.cd_servico = ip.id_item AND ip.tipo_item = 'Servico'
GROUP BY tipo_item, id_item, nome, cod_barras, valor_unitario
HAVING COUNT(*) > 5
ORDER BY qtd_ocorrencias DESC;


-- 8° Function e trigger para não permitir lançamento de produtos/serviços inativos no pedido


--8.1 Criar FUNCTION validar_ativo_item_pedido

CREATE OR REPLACE FUNCTION validar_ativo_item_pedido()
RETURNS TRIGGER AS $$
DECLARE
  ativo BOOLEAN;
BEGIN
  IF NEW.tipo_item = 'Produto' THEN
    SELECT ativo_produto INTO ativo
    FROM produto
    WHERE cd_produto = NEW.id_item;

    IF ativo IS DISTINCT FROM TRUE THEN
      RAISE EXCEPTION 'Produto inativo ou não existe';
    END IF;

  ELSIF NEW.tipo_item = 'Servico' THEN
    SELECT ativo_servico INTO ativo
    FROM servico
    WHERE cd_servico = NEW.id_item;

    IF ativo IS DISTINCT FROM TRUE THEN
      RAISE EXCEPTION 'Serviço inativo ou não existe';
    END IF;
  END IF;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;


--8.2 Criar TRIGGER trg_validar_ativo_item

CREATE TRIGGER trg_validar_ativo_item
BEFORE INSERT OR UPDATE ON item_pedido
FOR EACH ROW
EXECUTE FUNCTION validar_ativo_item_pedido();

-- 9° Criar objetos:

-- Criar FUNCTION criar_pedido

CREATE OR REPLACE FUNCTION criar_pedido(p_cliente_id INTEGER)
RETURNS INTEGER AS $$
DECLARE
    v_cd_pedido INTEGER;
BEGIN
    IF NOT EXISTS (SELECT 1 FROM cliente WHERE cd_cliente = p_cliente_id AND ativo_cliente = TRUE) THEN
        RAISE EXCEPTION 'Cliente inexistente ou inativo';
    END IF;

    INSERT INTO pedido (cliente_fk, situacao)
    VALUES (p_cliente_id, 'Em Andamento')
    RETURNING cd_pedido INTO v_cd_pedido;

    RETURN v_cd_pedido;
END;
$$ LANGUAGE plpgsql;

-- Criar FUNCTION registrar_pagamento

CREATE OR REPLACE FUNCTION registrar_pagamento(
    p_pedido_id INTEGER,
    p_fp TEXT,
    p_valor NUMERIC
)
RETURNS VOID AS $$
DECLARE
    v_cd_fp INTEGER;
    v_total_pago NUMERIC := 0;
    v_total_liquido NUMERIC := 0;
    v_qtd_pagamentos INTEGER := 0;
    v_situacao TEXT;
    v_valor_existente NUMERIC := 0;
BEGIN
    SELECT cd_fp INTO v_cd_fp FROM forma_pagamento WHERE descricao ILIKE p_fp;

    IF v_cd_fp IS NULL THEN
        RAISE EXCEPTION 'Forma de pagamento inválida';
    END IF;

    SELECT situacao INTO v_situacao FROM pedido WHERE cd_pedido = p_pedido_id;

    IF v_situacao IS NULL THEN
        RAISE EXCEPTION 'Pedido não encontrado';
    END IF;

    IF v_situacao = 'Pago' THEN
        RAISE EXCEPTION 'Pedido já está pago';
    END IF;


    SELECT valor_pago INTO v_valor_existente
    FROM pagamento_pedido
    WHERE pedido_fk = p_pedido_id AND forma_pagamento_fk = v_cd_fp;

    SELECT COALESCE(SUM(valor_pago), 0)
    INTO v_total_pago
    FROM pagamento_pedido
    WHERE pedido_fk = p_pedido_id;

    SELECT SUM((vl_unitario * quantidade) + vl_acrescimo - vl_desconto)
    INTO v_total_liquido
    FROM item_pedido
    WHERE pedido_fk = p_pedido_id;

    IF v_total_pago - v_valor_existente + v_valor_existente + p_valor > v_total_liquido THEN
        RAISE EXCEPTION 'Valor total pago ultrapassa o valor do pedido';
    END IF;

    IF FOUND THEN
        UPDATE pagamento_pedido
        SET valor_pago = valor_pago + p_valor
        WHERE pedido_fk = p_pedido_id AND forma_pagamento_fk = v_cd_fp;
    ELSE

        INSERT INTO pagamento_pedido (pedido_fk, forma_pagamento_fk, valor_pago)
        VALUES (p_pedido_id, v_cd_fp, p_valor);
    END IF;


    SELECT COALESCE(SUM(valor_pago), 0)
    INTO v_total_pago
    FROM pagamento_pedido
    WHERE pedido_fk = p_pedido_id;

    SELECT COUNT(*)
    INTO v_qtd_pagamentos
    FROM pagamento_pedido
    WHERE pedido_fk = p_pedido_id;

    IF v_total_pago = v_total_liquido AND v_qtd_pagamentos >= 3 THEN
        UPDATE pedido
        SET situacao = 'Pago'
        WHERE cd_pedido = p_pedido_id;
    END IF;
END;
$$ LANGUAGE plpgsql;



---- Criar FUNCTION adicionar_item

CREATE OR REPLACE FUNCTION adicionar_item(
    p_pedido_id INTEGER,
    p_tipo TEXT,
    p_id_item INTEGER,
    p_qtd INTEGER,
    p_vl_unit NUMERIC
)
RETURNS VOID AS $$
DECLARE
    v_situacao TEXT;
BEGIN
    SELECT situacao INTO v_situacao
    FROM pedido
    WHERE cd_pedido = p_pedido_id;

    IF v_situacao IS NULL THEN
        RAISE EXCEPTION 'Pedido não encontrado';
    ELSIF v_situacao = 'Pago' THEN
        RAISE EXCEPTION 'Não é permitido alterar pedidos pagos';
    END IF;

    IF EXISTS (
        SELECT 1 FROM item_pedido
        WHERE pedido_fk = p_pedido_id AND tipo_item = p_tipo AND id_item = p_id_item
    ) THEN
        UPDATE item_pedido
        SET quantidade = quantidade + p_qtd
        WHERE pedido_fk = p_pedido_id AND tipo_item = p_tipo AND id_item = p_id_item;

        DELETE FROM item_pedido
        WHERE pedido_fk = p_pedido_id AND tipo_item = p_tipo AND id_item = p_id_item AND quantidade <= 0;
    ELSE
        INSERT INTO item_pedido (
            pedido_fk, tipo_item, id_item, quantidade, vl_unitario
        ) VALUES (
            p_pedido_id, p_tipo, p_id_item, p_qtd, p_vl_unit
        );
    END IF;
END;
$$ LANGUAGE plpgsql;


-- Criar FUNCTION atualizar_acresc_desc

CREATE OR REPLACE FUNCTION atualizar_acresc_desc(
    p_cd_item INTEGER,
    p_perc_acrescimo NUMERIC DEFAULT 0,
    p_vl_acrescimo NUMERIC DEFAULT 0,
    p_perc_desconto NUMERIC DEFAULT 0,
    p_vl_desconto NUMERIC DEFAULT 0
)
RETURNS VOID AS $$
DECLARE
    v_pedido_id INTEGER;
    v_situacao TEXT;
BEGIN

    SELECT pedido_fk INTO v_pedido_id
    FROM item_pedido
    WHERE cd_item = p_cd_item;

    IF v_pedido_id IS NULL THEN
        RAISE EXCEPTION 'Item de pedido não encontrado';
    END IF;


    SELECT situacao INTO v_situacao
    FROM pedido
    WHERE cd_pedido = v_pedido_id;

    IF v_situacao = 'Pago' THEN
        RAISE EXCEPTION 'Não é permitido alterar acréscimos/descontos em pedidos pagos';
    END IF;

    UPDATE item_pedido
    SET 
        perc_acrescimo = p_perc_acrescimo,
        vl_acrescimo = p_vl_acrescimo,
        perc_desconto = p_perc_desconto,
        vl_desconto = p_vl_desconto
    WHERE cd_item = p_cd_item;
END;
$$ LANGUAGE plpgsql;


-- 10° Carga dos 10 pedidos 

DO $$
DECLARE
    p_id INT;
    prod_id INT;
    serv_id INT;
    produtos_ativos INT[] := ARRAY[1, 2, 4, 6, 7]; 
    servicos_ativos INT[] := ARRAY[1, 2];           
BEGIN
    FOR p_id IN 1..10 LOOP
        FOREACH prod_id IN ARRAY produtos_ativos LOOP
            PERFORM adicionar_item(p_id, 'Produto', prod_id, 2, 100.00 + prod_id);
        END LOOP;

        FOREACH serv_id IN ARRAY servicos_ativos LOOP
            PERFORM adicionar_item(p_id, 'Servico', serv_id, 1, 150.00 + serv_id);
        END LOOP;
    END LOOP;
END;
$$ LANGUAGE plpgsql;

-- 11° Carga do acrescimo/desconto 

DO $$
DECLARE
    item_rec RECORD;
BEGIN
    FOR item_rec IN
        SELECT ip.cd_item
        FROM item_pedido ip
        JOIN pedido p ON p.cd_pedido = ip.pedido_fk
        WHERE p.situacao = 'Em Andamento'
    LOOP
        PERFORM atualizar_acresc_desc(item_rec.cd_item, 5, 10, 2, 5);
    END LOOP;
END;
$$ LANGUAGE plpgsql;

-- 11° Carga do pagamento 

SELECT registrar_pagamento(8, 'Dinheiro', 459);
SELECT registrar_pagamento(8, 'Cartão de Crédito', 459);
SELECT registrar_pagamento(8, 'Pix', 460);

SELECT registrar_pagamento(9, 'Boleto', 459);
SELECT registrar_pagamento(9, 'Cartão de Débito', 459);
SELECT registrar_pagamento(9, 'Pix', 460);

SELECT registrar_pagamento(10, 'Dinheiro', 459.00);
SELECT registrar_pagamento(10, 'Cartão de Crédito', 459);
SELECT registrar_pagamento(10, 'Cartão de Débito', 460);
