FROM fidelissauro/apko-go:latest-amd64 AS builder

WORKDIR /root/src/app

# Copiar apenas os arquivos de dependência primeiro para cache eficiente
COPY go.mod go.sum ./

# Baixar as dependências antes de copiar o código-fonte
RUN go mod tidy

# Agora copia o restante do código
COPY . ./

# Compilar o binário de forma otimizada para Linux
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Imagem final
FROM fidelissauro/apko-run:latest-amd64

# Copia apenas o binário para a imagem final
COPY --from=builder /root/src/app/main ./

EXPOSE 8080

ENTRYPOINT ["./main"]