# Django Async vs Sync Views

Um projeto Django simples que demonstra a diferença entre **views síncronas** e **assíncronas**, mostrando como cada uma lida com operações bloqueantes.

## Sobre o Projeto

Este projeto foi criado para demonstrar:
- Como views síncronas bloqueiam o servidor durante operações I/O
- Como views assíncronas permitem que o servidor continue processando outras requisições
- A diferença de performance entre os dois abordagens

## Tecnologias

- **Django 5.2.5**
- **Python 3.13**
- **httpx** - Para requisições HTTP
- **asyncio** - Para programação assíncrona

## Estrutura do Projeto

```
django-async-vs-sync/
├── asyncviews/          # Aplicação principal
│   ├── views.py        # Views síncronas e assíncronas
│   ├── urls.py         # Configuração de URLs
│   ├── settings.py     # Configurações do Django
│   └── asgi.py         # Configuração ASGI
├── manage.py           # Script de gerenciamento Django
└── README.md           # Este arquivo
```

## 🔧 Instalação

1. **Clone o repositório:**
   ```bash
   git clone <url-do-repositorio>
   cd django-async-vs-sync
   ```

2. **Ative o ambiente virtual:**
   ```bash
   # Windows
   env\Scripts\activate
   
   # Linux/Mac
   source env/bin/activate
   ```

3. **Instale as dependências:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Execute as migrações:**
   ```bash
   python manage.py migrate
   ```

5. **Inicie o servidor:**
   ```bash
   python manage.py runserver
   ```

## Endpoints Disponíveis

- **`/api/`** - API simples com delay de 1 segundo
- **`/sync_view/`** - View síncrona (bloqueante)
- **`/async_view/`** - View assíncrona (não-bloqueante)
- **`/admin/`** - Interface administrativa do Django

## Comparação: Sync vs Async

### View Síncrona (`/sync_view/`)
- Bloqueia o servidor durante a execução
- Processa uma requisição por vez
- Delay de 5 segundos + requisição HTTP
- **Resultado:** Servidor fica indisponível para outras requisições

### View Assíncrona (`/async_view/`)
- Não bloqueia o servidor
- Permite múltiplas requisições simultâneas
- Mesmo delay de 5 segundos + requisição HTTP
- **Resultado:** Servidor continua respondendo outras requisições

## Testando

1. **Teste a view síncrona:**
   ```bash
   curl http://localhost:8000/sync_view/
   ```

2. **Teste a view assíncrona:**
   ```bash
   curl http://localhost:8000/async_view/
   ```

3. **Compare o comportamento:**
   - Abra múltiplas abas do navegador
   - Acesse `/sync_view/` em uma e `/api/` em outra
   - Observe que `/api/` fica bloqueado até `/sync_view/` terminar

## Código de Exemplo

### View Síncrona
```python
def sync_view(request):
    http_call_sync()  # Bloqueia por 5 segundos
    return HttpResponse("Blocking HTTP request")
```

### View Assíncrona
```python
async def async_view(request):
    loop = asyncio.get_event_loop()
    loop.create_task(http_call_async())  # Não bloqueia
    return HttpResponse("Non-blocking HTTP request")
```

## Aprendizados

- **Views síncronas** são mais simples de entender e debugar
- **Views assíncronas** oferecem melhor performance para I/O intensivo
- Django suporta ambos os padrões
- Use **async** quando precisar de concorrência real
- Use **sync** para operações simples e diretas

## Recursos Adicionais

- [Documentação do Django](https://docs.djangoproject.com/)
- [Python asyncio](https://docs.python.org/3/library/asyncio.html)
- [httpx - HTTP client](https://www.python-httpx.org/)

## Contribuição

Sinta-se à vontade para contribuir com melhorias, correções ou exemplos adicionais!

---

**Desenvolvido para fins educacionais**