# sshd_server/Dockerfile
FROM ubuntu:22.04

# コンテナ内で非対話的にパッケージをインストールするための設定
ENV DEBIAN_FRONTEND=noninteractive 


# openssh-serverとsupervisorのインストール
RUN apt update && \
    apt install -y openssh-server supervisor && \
    apt clean && \
    # aptのキャッシュを削除
    rm -rf /var/lib/apt/lists/*


# SSHの設定を置換
# SSH接続時にパスワード認証を有効にする
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
# rootユーザーでのログインを許可
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config


# passwdは対話型なので、chpasswdでrootユーザーのパスワードを設定
RUN echo 'root:password' | chpasswd


# supervisorの設定
# supervisor.confをコピー
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf


# SSH用に22番ポートを公開
EXPOSE 22


# sshサーバーを起動
# supervisorを使うとsshdが落ちても再起動してくれる！・・・らしい
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]