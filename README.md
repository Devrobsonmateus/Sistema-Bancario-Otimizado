# Sistema-Bancario-Otimizado
#Desafio Dio otimizado do sistema bancario basíco 
import textwrap


def menu():
    menu = """\n
    ======== BANCO RMG ========
    
    1 - \tSAQUE
    2 - \tDEPOSITO 
    3 - \tEXTRATO
    4 - \tNOVA CONTA
    5 - \tLISTAR CONTAS
    6 - \tNOVO USUÁRIO
    0 - \tSAIR 
   
    ===========================

    ->  """
    return input(textwrap.dedent(menu))


def sacar(*, saldo, valor, extrato, limite, quantidade_saque, limite_saque, LIMITE_SAQUE):
    limite_saque  = quantidade_saque >= LIMITE_SAQUE
    limite_saldo = valor > saldo
    limite_diaria = valor >  limite

    if limite_saque:
        print('\n          LIMITE DE SAQUES EXCEDIDO! \nVOLTE NOVAMENTE AMANHÃ PARA UMA NOVA TRANSAÇÃO.')
        
    elif limite_saldo:
        print("\nSALDO INSUFICIENTE!")
            
    elif limite_diaria:
        print('\n      LIMITE DE SAQUE DIARO EXCEDIDO! \nVOLTE NOVAMENTE AMANHÃ PARA UMA NOVA TRANSAÇÃO.')
            

    elif valor > 0:
        saldo -= valor 
        extrato += f"SAQUE: R$ {valor:.2f}\n"
        quantidade_saque += 1
        mensagem_saque = "SAQUE REALIZADO COM SUCESSO!"
        print()
        print(mensagem_saque.center(38))

    else:
        print('AGUARDE A CONTAGEM DAS CÉDULAS \nSAQUE REALIADO COM SUCESSO!!!')    


def deposito(saldo, valor, extrato, /):
     if valor < 0:
            print("\nVALOR PARA DEPÓSITO INVÁLIDO.")
     elif valor > 0:
            mensagem = "DEPOSITO REALIZADO COM SUCESSO!"
            print(mensagem.center(38))
            saldo += valor
            extrato += f'DEPOSITO R$:  {valor:.2f}\n'

     return saldo, extrato


def exibir_extrato(saldo,/,*,extrato):
     print("============== BANCO RMG  ============")
     print()
     print("     ========== EXTRATO ==========")
     print()
     nova_mensagem = "NÃO FORAM REALIZADAS MOVIMENTAÇÕES."
     print(nova_mensagem.center(40) if not extrato else extrato)
     print(f"\nSALDO: R$ {saldo:.2f}")
     print("==========================================")
     

def criar_usuario(usuarios):
     cpf = input("Informe o CPF (somente número): ")
     usuario = filtrar_usuario(cpf, usuarios)

     if usuario:
        print("\n@@@ Já existe usuário com esse CPF! ")
        return

     nome = input("Informe o nome completo: ")
     data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
     endereco = input("Informe o endereço (logradouro, nro - bairro - cidade/sigla estado): ")

     usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})
 
     print("=== Usuário criado com sucesso! ===")


def filtrar_usuario(cpf,usuarios):
     usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf]
     return usuarios_filtrados[0] if usuarios_filtrados else None


def criar_conta(agencia, numero_conta, usuarios):
     cpf = input("Informe o CPF do usuário: ")
     usuario = filtrar_usuario(cpf, usuarios)

     if usuario:
        print("\n=== Conta criada com sucesso! ===")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

     print("\n@@@ Usuário não encontrado, fluxo de criação de conta encerrado! @@@")


def listrar_contas(contas):
     for conta in contas:
        linha = f"""\
            Agência:\t{conta['agencia']}
            C/C:\t\t{conta['numero_conta']}
            Titular:\t{conta['usuario']['nome']}
        """
        print("=" * 100)
        print(textwrap.dedent(linha))

def main():
     saldo = 0
     extrato = "" 
     limite = 500
     quantidade_saque = 0
     usuarios =[]
     contas = []

     LIMITE_SAQUE = 3
     AGENCIA = "0001"


     while True:
        opcao = menu() 

        if opcao  == "1": #saque
               valor = float(input("\nINFORME O VALOR DESEJADO PARA SAQUE: \nR$: "))
               
               saldo, extrato = sacar(
                    saldo=saldo,
                    valor=valor,
                    extrato=extrato,
                    limite=limite,
                    quantidade_saque=quantidade_saque,
                    limite_saques=LIMITE_SAQUE,
                )
             
        elif opcao == "2": 
            valor = float(input("\nINFORME O VALOR DESEJADO PARA DEPOSITO.\nR$: "))
        
            saldo, extrato = deposito(saldo, valor, extrato)      
       
        elif opcao == "3": 
             print()
        
        elif opcao == "4":
             numero_conta = len(contas) + 1
             conta = criar_conta(AGENCIA, numero_conta, usuarios)

             if conta:
                contas.append(conta)

        elif opcao == "5":
             listrar_contas(contas)

        elif opcao == "6":
             criar_usuario(usuarios)


        elif opcao == "0": #sair
             break
  
        else:
              print("OPERAÇÃO INVÁLIDA. \nPor favor selicione novamente a operação desejada!")


main()


    



