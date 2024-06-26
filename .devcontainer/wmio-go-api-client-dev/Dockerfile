ARG __golang_image=golang:1.22.3-alpine3.19
ARG __baseImage=alpine:3.19
FROM ${__golang_image} as golang-image 

FROM ${__baseImage}
RUN apk update --no-cache \
    apk upgrade --no-cache
    
ARG __gid=1000
ARG __gname=vscode
ARG __uid=1000
ARG __uname=vscode

RUN apk update --no-cache \
    apk upgrade --no-cache \
    && apk add --no-cache \
      ca-certificates \
      git \
    && addgroup -g ${__gid} ${__gname} \
    && adduser -u ${__uid} -G ${__gname} --disabled-password ${__uname}

COPY --from=golang-image /usr/local/go/ /usr/local/go/
ENV PATH="${PATH}:/usr/local/go/bin"
RUN wget -O- -nv https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v1.58.1 

USER ${__uname}

RUN go install -v github.com/go-delve/delve/cmd/dlv@latest \
    && go install golang.org/x/tools/cmd/godoc@latest \
    && go install golang.org/x/tools/gopls@latest \
    && go install honnef.co/go/tools/cmd/staticcheck@latest
ENV PATH="${PATH}:/home/${__uname}/go/bin/"
