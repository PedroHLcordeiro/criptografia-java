# criptografia-java
import org.mindrot.jbcrypt.BCrypt;
import java.util.Scanner;

public class SecureMessagingExample {

    public static void main(String[] args) {
        // Exemplo de uso
        String mensagemOriginal = "Esta é uma mensagem confidencial.";

        // Gere um par de chaves
        String[] keyPair = generateKeyPair();

        // Criptografe a mensagem
        String mensagemCriptografada = encryptMessage(keyPair[0], mensagemOriginal);
        System.out.println("Mensagem Criptografada: " + mensagemCriptografada);

        // Solicite a senha mestra antes de tentar decifrar
        System.out.print("Digite sua senha mestra para decifrar a mensagem: ");
        String senhaMestraDigitada = new Scanner(System.in).nextLine();

        // Decifre a mensagem apenas se a senha mestra estiver correta
        String mensagemDecifrada = decryptMessage(keyPair[1], mensagemCriptografada, senhaMestraDigitada);
        System.out.println("Resultado da Decifragem: " + mensagemDecifrada);
    }

    static String[] generateKeyPair() {
        // Gere uma senha aleatória (para fins de exemplo)
        String senha = promptForPassword("Digite sua senha: ");

        // Gere um "salt" aleatório para cada usuário (deve ser armazenado junto com a senha no mundo real)
        String salt = BCrypt.gensalt();

        // Gere o hash da senha usando BCrypt
        String hashSenha = BCrypt.hashpw(senha, salt);

        // Solicite a senha mestra (para fins de exemplo)
        String senhaMestra = promptForPassword("Digite sua senha mestra: ");

        // Gere o hash da senha mestra usando BCrypt
        String hashSenhaMestra = BCrypt.hashpw(senhaMestra, salt);

        return new String[]{hashSenha, hashSenhaMestra};
    }

    static String encryptMessage(String key, String message) {
        // Este exemplo usa BCrypt apenas para fins de ilustração, mas BCrypt é projetado para senhas, não para cifrar mensagens.
        // Em um caso real, você usaria um algoritmo de cifragem simétrica adequado para mensagens.

        // Você pode armazenar a chave em um local seguro (por exemplo, usando um KMS) para uso futuro.

        return BCrypt.hashpw(message, key);
    }

    static String decryptMessage(String key, String encryptedMessage, String senhaMestra) {
        // Este exemplo usa BCrypt apenas para fins de ilustração, mas BCrypt é projetado para senhas, não para decifrar mensagens.
        // Em um caso real, você usaria um algoritmo de cifragem simétrica adequado para mensagens.

        // Recupere a chave do local seguro (por exemplo, usando um KMS) se necessário.

        // Você precisaria implementar a lógica de decifragem apropriada para o algoritmo de cifragem simétrica escolhido.

        // Verifique se a senha mestra está correta antes de decifrar
        if (BCrypt.checkpw(senhaMestra, key)) {
            return "Mensagem Decifrada";
        } else {
            return "Falha na decifragem. Senha mestra incorreta.";
        }
    }

    static String promptForPassword(String prompt) {
        System.out.print(prompt);
        return new Scanner(System.in).nextLine();
    }
}
