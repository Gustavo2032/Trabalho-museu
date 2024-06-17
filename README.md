import pickle

class ObraDeArte:
    def __init__(self, titulo, data_criacao, tema, estilo, descricao, tecnica, autor, localizacao):
        self.titulo = titulo
        self.data_criacao = data_criacao
        self.tema = tema
        self.estilo = estilo
        self.descricao = descricao
        self.tecnica = tecnica
        self.autor = autor
        self.localizacao = localizacao
        self.documentos = []
        self.figura_proeminente = None

    def adicionar_documento(self, documento):
        self.documentos.append(documento)

    def __repr__(self):
        return f"<ObraDeArte '{self.titulo}' por {self.autor}>"

class Artista:
    def __init__(self, nome, data_nascimento, local_nascimento, biografia):
        self.nome = nome
        self.data_nascimento = data_nascimento
        self.local_nascimento = local_nascimento
        self.biografia = biografia
        self.estilos = []

    def adicionar_estilo(self, estilo):
        self.estilos.append(estilo)

    def __repr__(self):
        return f"<Artista {self.nome}>"

class EstiloArtistico:
    def __init__(self, denominacao, periodo, caracteristicas, escola_representativa):
        self.denominacao = denominacao
        self.periodo = periodo
        self.caracteristicas = caracteristicas
        self.escola_representativa = escola_representativa

    def __repr__(self):
        return f"<Estilo {self.denominacao}>"

class EventoExterno:
    def __init__(self, periodo_emprestimo, nome_evento, responsavel, tema):
        self.periodo_emprestimo = periodo_emprestimo
        self.nome_evento = nome_evento
        self.responsavel = responsavel
        self.tema = tema

    def __repr__(self):
        return f"<EventoExterno '{self.nome_evento}'>"

class VisitaGuiada:
    def __init__(self, tema, descricao, obras):
        self.tema = tema
        self.descricao = descricao
        self.obras = obras

    def __repr__(self):
        return f"<VisitaGuiada '{self.tema}'>"


def ordenar_obras_por_titulo(obras):
    return sorted(obras, key=lambda obra: obra.titulo)

def ordenar_artistas_por_nome(artistas):
    return sorted(artistas, key=lambda artista: artista.nome)

def ordenar_estilos_por_periodo(estilos):
    return sorted(estilos, key=lambda estilo: estilo.periodo)


def salvar_dados(nome_arquivo, dados):
    with open(nome_arquivo, 'wb') as f:
        pickle.dump(dados, f)

def carregar_dados(nome_arquivo):
    try:
        with open(nome_arquivo, 'rb') as f:
            return pickle.load(f)
    except FileNotFoundError:
        return None
    except Exception as e:
        print(f"Erro ao carregar dados: {e}")
        return None


def mostrar_detalhes_obra(obra):
    print(f"\nDetalhes da obra selecionada:")
    print(f"Título: {obra.titulo}")
    print(f"Autor: {obra.autor}")
    print(f"Data de Criação: {obra.data_criacao}")
    print(f"Tema: {obra.tema}")
    print(f"Estilo: {obra.estilo}")
    print(f"Técnica: {obra.tecnica}")
    print(f"Localização: {obra.localizacao}")
    print(f"Descrição: {obra.descricao}")

    if obra.documentos:
        print("Documentos:")
        for documento in obra.documentos:
            print(documento)
    else:
        print("Sem documentos adicionados.")

def main():
    obra1 = ObraDeArte("Mona Lisa", "1503-1506", "Retrato", "Renascimento", "Famoso retrato de Lisa Gherardini", "Óleo sobre madeira", "Leonardo da Vinci", "Sala 4")
    obra2 = ObraDeArte("Guernica", "1937", "Histórico", "Cubismo", "Pintura de Pablo Picasso sobre a Guerra Civil Espanhola", "Óleo sobre tela", "Pablo Picasso", "Sala 7")
    artista1 = Artista("Leonardo da Vinci", "15 de abril de 1452", "Vinci, Itália", "Pintor, escultor, cientista, engenheiro")
    estilo1 = EstiloArtistico("Renascimento", "Séculos XIV-XVI", "Valorização do racionalismo, perspectiva e proporção", "Escola Florentina")
    evento1 = EventoExterno("Junho 2024", "Exposição Internacional de Arte Moderna", "Maria Silva", "Arte Moderna")
    visita1 = VisitaGuiada("Obras do Renascimento", "Visita guiada pelas obras de Leonardo da Vinci e Michelangelo", [obra1])

    artista1.adicionar_estilo(estilo1)

    obras = [obra1, obra2]
    artistas = [artista1]
    estilos = [estilo1]
    eventos = [evento1]
    visitas = [visita1]

    obras_ordenadas = ordenar_obras_por_titulo(obras)
    artistas_ordenados = ordenar_artistas_por_nome(artistas)
    estilos_ordenados = ordenar_estilos_por_periodo(estilos)

    salvar_dados('museu_dados.bin', (obras, artistas, estilos, eventos, visitas))

    dados_carregados = carregar_dados('museu_dados.bin')
    if dados_carregados:
        obras_carregadas, artistas_carregados, estilos_carregados, eventos_carregados, visitas_carregadas = dados_carregados
        print("Dados carregados com sucesso!")
        print(obras_carregadas)
        print(artistas_carregados)
        print(estilos_carregados)
        print(eventos_carregados)
        print(visitas_carregadas)
    else:
        print("Não foi possível carregar os dados.")

    while True:
        print("\nMenu de Obras:")
        print("1. Mostrar detalhes da obra")
        print("2. Sair")

        escolha = input("Escolha uma opção: ")

        if escolha == '1':
            print("\nLista de obras disponíveis:")
            for i, obra in enumerate(obras):
                print(f"{i+1}. {obra.titulo}")

            try:
                indice = int(input("Escolha o número da obra para ver detalhes: "))  - 1
                if 0 <= indice < len(obras):
                    obra_selecionada = obras[indice]
                    mostrar_detalhes_obra(obra_selecionada)
                else:
                    print("Opção inválida. Escolha um número válido da lista.")
            except ValueError:
                print("Por favor, digite um número válido.")

        elif escolha == '2':
            print("Saindo do programa...")
            break

        else:
            print("Opção inválida. Escolha novamente.")


if __name__ == "__main__":
    main()

