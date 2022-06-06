package readability;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.Scanner;


public class Main {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        String text = Files.readString(Path.of(args[0]));
        String[] words = text.split("\\s+");

        int sentences = text.split("[.!?]").length;
        int wordCount = words.length;
        int chars = text.replaceAll("\\s", "").length();
        int syllables = 0;
        for (String word : words) {
            syllables += countSyl(word);
        }
        int polySyl = 0;
        for (String word : words) {
            if (isPolySyl(word)) {
                polySyl++;
            }
        }

        printTextInfo(text, chars, syllables, polySyl, wordCount, sentences);
        System.out.print("Enter the score you want to calculate (ARI, FK, SMOG, CL, all): ");
        String input = sc.nextLine().toUpperCase();
        System.out.println("\n");

        double score;
        switch (input) {
            case "ARI":
                score = getARI(chars, wordCount, sentences);
                System.out.println("Automated Readability Index: " + score + " (about " + getAge(score) + "-year-olds).\n");
                break;
            case "FK":
                score = getFK(syllables, wordCount, sentences);
                System.out.println("Flesch–Kincaid readability tests: " + score + " (about " + getAge(score) + "-year-olds).\n");
                break;
            case "SMOG":
                score = getSMOG(polySyl, sentences);
                System.out.println("Simple Measure of Gobbledygook: " + score + " (about " + getAge(score) + "-year-olds).\n");
                break;
            case "CL":
                score = getCL(chars, wordCount, sentences);
                System.out.println("Coleman–Liau index: " + score + " (about " + getAge(score) + "-year-olds).\n");
                break;
            case "ALL":
                double score1 = getARI(chars, wordCount, sentences);
                System.out.println("Automated Readability Index: " + score1 + " (about " + getAge(score1) + "-year-olds).");
                double score2 = getFK(syllables, wordCount, sentences);
                System.out.println("Flesch–Kincaid readability tests: " + score2 + " (about " + getAge(score2) + "-year-olds).");
                double score3 = getSMOG(polySyl, sentences);
                System.out.println("Simple Measure of Gobbledygook: " + score3 + " (about " + getAge(score3) + "-year-olds).");
                double score4 = getCL(chars, wordCount, sentences);
                System.out.println("Coleman–Liau index: " + score4 + " (about " + getAge(score4) + "-year-olds).\n");
                System.out.println("This text should be understood in average by " + ((score1 + score2 + score3 + score4) / 4) + "-year-olds.");
        }
    }

    static double getARI(int chars, int wordCount, int sentences) {
        return  4.71 * ((double) chars / wordCount) + 0.5 * ((double) wordCount / sentences) - 21.43;
    }

    static double getFK(int syllables, int wordCount, int sentences) {
        return 11.8 * ((double) syllables / wordCount) + 0.39 * ((double) wordCount / sentences) - 15.59;
    }

    static double getSMOG(int polySyl, int sentences) {
        return 1.043 * Math.sqrt(polySyl * ((double) 30 / sentences)) - 3.1291;
    }

    static double getCL(int chars, int wordCount, int sentences) {
        return 5.88 * ((double) chars / wordCount) - 29.6 * ((double) sentences / wordCount) - 15.8;
    }

    static void printTextInfo(String text, int chars, int syllables, int polySyl, int wordCount, int sentences) {

        System.out.println("The text is:\n" + text + "\n\n");
        System.out.println("Words: " + wordCount + "\n" +
                "Sentences: " + sentences + "\n" +
                "Characters: " + chars + "\n" +
                "Syllables: " + syllables + "\n" +
                "Polysyllables: " + polySyl + "\n");
    }

    static int getAge(double score) {
        int ceiledScore = (int) Math.ceil(score);
        if (ceiledScore < 3) {
            return ceiledScore + 5;
        }
        if (ceiledScore < 13) {
            return ceiledScore + 6;
        }
        return 24;
    }

    static boolean isPolySyl(String word) {
        return countSyl(word) >= 3;
    }

    static int countSyl(String word) {
        int count = word.split("[aeiouyAEIOUY]+").length;
        // Above divides into groups of consonants (including an empty first string for words starting with vowels),
        // all of which except possibly the last must precede a group of vowels i.e. a syllable.
        // We then check the last character to see if it is a vowel (except e) and if not, subtract the extra syllable.
        if (!word.substring(word.length() - 1).matches("[aiouyAIOUY]")) {
            count--;
        }
        return count;
    }
}
