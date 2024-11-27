//EXO POUR ENVOYER PLUSIEURS REQUETES
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.BufferedReader;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class BasicServer {
    static int SERVER_PORT = 5000;

    public static void main(String[] args) {
        try (ServerSocket server = new ServerSocket(SERVER_PORT)) {
            System.out.println("Le serveur a démarré sur le port : " + SERVER_PORT);

            while (true) {
                Socket socket = server.accept();
                System.out.println("Client connecté : " + socket.getRemoteSocketAddress());
                
                try (InputStream is = socket.getInputStream();
                     InputStreamReader isr = new InputStreamReader(is);
                     BufferedReader input = new BufferedReader(isr);
                     OutputStream os = socket.getOutputStream();
                     PrintWriter output = new PrintWriter(os, true)) {

                    String message;
                    while ((message = input.readLine()) != null) {
                        System.out.println("Message reçu : " + message);
                        output.println("Message reçu : " + message);

                        // Arrêt du serveur si le message est "by"
                        if (message.trim().equalsIgnoreCase("by")) {
                            System.out.println("Commande 'by' reçue, arrêt du serveur...");
                            socket.close();
                            server.close();
                            System.exit(0);
                        }
                    }
                } catch (IOException e) {
                    System.out.println("Erreur avec le client : " + e.getMessage());
                }
            }
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}


//EXO POUR CAPITALISER UN MESSAGE
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.BufferedReader;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class Capitaliser {
    static int SERVER_PORT = 5000;

    public static void main(String[] args) {
        try (ServerSocket server = new ServerSocket(SERVER_PORT)) {
            System.out.println("Le serveur a démarré sur le port : " + SERVER_PORT);

            while (true) {
                Socket socket = server.accept();
                System.out.println("Client connecté : " + socket.getRemoteSocketAddress());

                try (InputStream is = socket.getInputStream();
                     InputStreamReader isr = new InputStreamReader(is);
                     BufferedReader input = new BufferedReader(isr);
                     OutputStream os = socket.getOutputStream();
                     PrintWriter output = new PrintWriter(os, true)) {

                    String message;
                    while ((message = input.readLine()) != null) {
                        String capitalizedMessage = message.toUpperCase();
                        System.out.println("Message reçu : " + capitalizedMessage);
                        output.println("Message capitalisé : " + capitalizedMessage);

                        if (capitalizedMessage.equalsIgnoreCase("BY")) {
                            System.out.println("Commande 'BY' reçue, arrêt du serveur...");
                            socket.close();
                            server.close();
                            System.exit(0);
                        }
                    }
                } catch (IOException e) {
                    System.out.println("Erreur avec le client : " + e.getMessage());
                }
            }
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}

//EXO CALCULATRICE 

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class Calculatrice {

        static int SERVER_PORT = 5000;

        public static void main(String[] args) {
            try (ServerSocket server = new ServerSocket(SERVER_PORT)) {
                System.out.println("Le serveur a démarré sur le port : " + SERVER_PORT);

                while (true) {
                    Socket socket = server.accept();
                    System.out.println("Client connecté : " + socket.getRemoteSocketAddress());

                    try (BufferedReader input = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                         PrintWriter output = new PrintWriter(socket.getOutputStream(), true)) {

                        String expression;
                        while ((expression = input.readLine()) != null) {
                            System.out.println("Expression reçue : " + expression);

                            // Calculer le résultat de l'expression
                            String resultat = calculer(expression);

                            // Envoyer le résultat au client
                            output.println("Résultat : " + resultat);
                        }
                    } catch (IOException e) {
                        System.out.println("Erreur avec le client : " + e.getMessage());
                    }
                }
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }

        // Méthode pour calculer l'expression
        private static String calculer(String expression) {
            try {
                // Supprime les espaces et évalue l'expression en utilisant JavaScript (pour simplifier)
                expression = expression.replaceAll("\\s+", "");
                double result = (double) new javax.script.ScriptEngineManager()
                        .getEngineByName("JavaScript")
                        .eval(expression, new javax.script.SimpleBindings(new java.util.HashMap<>()));

                return String.valueOf(result);
            } catch (Exception e) {
                return "Erreur de calcul : " + e.getMessage();
            }
        }
    }
