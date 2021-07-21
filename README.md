If you want to authenticate with Vault using AWS IAM from your java application, you can use AwsIamLogin.java defined in this GIST. It is based on https://github.com/BetterCloud/vault-java-driver/issues/118#issuecomment-427731009.

With https://github.com/BetterCloud/vault-java-driver imported, you can acquire a token as follows:

final VaultConfig config = new VaultConfig().engineVersion(2)
                                            .address(url)
                                            .sslConfig(new SslConfig().build());
final Vault authVault = new Vault(config);
final AuthResponse authResponse = authVault.auth()
                                           .loginByAwsIam(
                                                   role,
                                                   AwsIamLogin.INSTANCE.getBase64EncodedRequestUrl(),
                                                   AwsIamLogin.INSTANCE.getBase64EncodedRequestBody(),
                                                   AwsIamLogin.INSTANCE.getBase64EncodedRequestHeaders(url),
                                                   AwsIamLogin.INSTANCE.getAuthMount()
                                           );
final String token = authResponse.getAuthClientToken();
