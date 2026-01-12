import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Scanner;
import java.util.Set;

/**
 * Program reads a txt file and outputs an HTML file with each word's
 * repetition. unique by font and size
 *
 * @author Hyunmin and Simran
 *
 */
public final class TagCloudGeneratorStandard {
    /**
     * Minimum font sizes.
     */
    private static final int MIN_FONT_SIZE = 11;
    /**
     * Maximum font sizes.
     */
    private static final int MAX_FONT_SIZE = 48;

    /**
     * Compare in alphabetical order by keys.
     */
    private static final class OrderByWord
            implements Comparator<Map.Entry<String, Integer>> {

        // String comparator
        @Override
        public int compare(Map.Entry<String, Integer> one,
                Map.Entry<String, Integer> two) {

            return one.getKey().toLowerCase().compareTo(two.getKey().toLowerCase());
        }
    }

    /**
     * Integer comparator by decreasing order by values.
     */
    private static final class OrderByCount
            implements Comparator<Map.Entry<String, Integer>> {
        @Override
        public int compare(Map.Entry<String, Integer> one,
                Map.Entry<String, Integer> two) {
            return two.getValue().compareTo(one.getValue());
        }
    }

    /**
     *
     */
    private TagCloudGeneratorStandard() {
    }

    /**
     * Selects the top {@code nWords} most frequent words from the given map and
     * returns them as a list sorted alphabetically.
     *
     * <p>
     * The method performs two steps:
     * <ol>
     * <li>Sorts all (word, count) pairs in descending order of count, then
     * keeps only the top {@code nWords} entries.</li>
     * <li>Sorts those selected entries alphabetically by word
     * (case-insensitive) before returning them.</li>
     * </ol>
     *
     * @param words
     *            a {@code Map<String, Integer>} containing words and their
     *            counts
     * @param nWords
     *            the number of words to include in the final list; must be > 0
     * @return a {@code List<Map.Entry<String, Integer>>} containing up to
     *         {@code nWords} entries, sorted alphabetically by word
     */
    private static List<Map.Entry<String, Integer>> sort(Map<String, Integer> words,
            int nWords) {
        assert words != null : "Violation of: words is not null";

        List<Map.Entry<String, Integer>> wordList = new ArrayList<>(words.entrySet());
        // sort by count
        Collections.sort(wordList, new OrderByCount());
        // check to assure use of the top nWords
        if (wordList.size() > nWords) {
            wordList = wordList.subList(0, nWords);
        }
        // sort in alphabetical order
        Collections.sort(wordList, new OrderByWord());

        return wordList;

    }

    /**
     * Populates {@code set} with all distinct characters from a {@code str}.
     *
     * @param str
     *            characters to insert
     * @param set
     *            destination set
     */
    private static void generateElements(String str, Set<Character> set) {
        assert str != null : "Violation of: str is not null";
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            //Adds new character into set
            if (!set.contains(ch)) {
                set.add(ch);
            }
        }
    }

    /**
     * Returns the next token in the string starting at {@code position}. A
     * token is either a maximal-length word (characters not in
     * {@code separators}) or a maximal-length separator string (characters in
     * {@code separators}).
     *
     * @param text
     *            the input string
     * @param position
     *            starting index in {@code text}; must satisfy 0 ≤ position <
     *            text.length()
     * @param separators
     *            the set of separator characters
     * @return the next word or separator substring beginning at
     *         {@code position}
     * @throws IllegalArgumentException
     *             if {@code position} is out of range
     */
    private static String nextWordOrSeparator(String text, int position,
            Set<Character> separators) {
        assert text != null : "Violation of: text is not null";
        assert separators != null : "Violation of: separators is not null";
        assert 0 <= position : "Violation of: 0 <= position";
        assert position < text.length() : "Violation of: position < |text|";

        char first = text.charAt(position);
        boolean isSep = separators.contains(first);

        int i = position;
        while (i < text.length() && (separators.contains(text.charAt(i)) == isSep)) {
            i++;
        }
        return text.substring(position, i);
    }

    /**
     * Increments the count for {@code word} in {@code counts} or add the key
     * and count if it was not present.
     *
     * @param counts
     *            map with word counts
     * @param word
     *            key to increment
     */
    private static void wordCounter(Map<String, Integer> counts, String word) {
        if (!counts.containsKey(word)) {
            counts.put(word, 1);
            // add with count 1 if new, otherwise increment existing count
        } else {
            int x = counts.get(word);
            counts.put(word, x + 1);
        }
    }

    /**
     * Reads the entire input texts and updates the word counts map.
     *
     * @param in
     *            reader (at start of file)
     * @param counts
     *            destination word counts
     * @param separators
     *            set of separator characters
     */
    private static void wordIntoMap(BufferedReader in, Map<String, Integer> counts,
            Set<Character> separators) throws IOException {
        String line = in.readLine();
        while (line != null) {
            // store whole characters of text file into one string.
            int pos = 0;
            while (pos < line.length()) {
                // scan for either word or separator by going through each character
                String token = nextWordOrSeparator(line, pos, separators);
                // count if it is a word
                if (!separators.contains(token.charAt(0))) {
                    String word = token.toLowerCase(); // convert to lower case
                    wordCounter(counts, word);
                }
                pos += token.length();
            }
            line = in.readLine();
        }
    }

    /**
     * Writes the header HTML for a page.
     *
     * @param out
     *            destination writer (open)
     * @param inputPath
     *            original input file path
     * @param nWords
     *            number of words in tag cloud
     **/
    private static void outputIndexHeader(PrintWriter out, String inputPath, int nWords) {
        String title = "Top " + nWords + " Words in " + inputPath;

        out.println("<html>");
        out.println("<head>");
        out.println("<meta charset=\"utf-8\" />");
        out.println("<title>" + title + "</title>");
        out.println("<link href=\"http://web.cse.ohio-state.edu/software/2231"
                + "/web-sw2/assignments/projects/tag-cloud-generator/data/"
                + "tagcloud.css\" rel=\"stylesheet\" type=\"text/css\">");
        out.println("<link href=\"tagcloud.css\" rel=\"stylesheet\" type=\"text/css\">");
        out.println("</head>");
        out.println("<body>");
        out.println("<h2>" + title + "</h2>");
        out.println("<hr>");
        out.println("<div class=\"cdiv\">");
        out.println("<p class=\"cbox\">");
    }

    /**
     * Computes font size between MIN_FONT_SIZE and MAX_FONT_SIZE based on
     * count, given the minimum and maximum counts over all words.
     *
     * @param count
     *            count for the current word
     * @param min
     *            minimum count among all words in the tag cloud
     * @param max
     *            maximum count among all words in the tag cloud
     * @return font size to use for this word
     *
     */
    private static int computeFontSize(int count, int min, int max) {
        int size;
        if (max == min) {
            // All words have the same count: use middle font size
            size = (MIN_FONT_SIZE + MAX_FONT_SIZE) / 2;
        } else {
            double scale = (double) (count - min) / (double) (max - min);
            size = MIN_FONT_SIZE
                    + (int) Math.round(scale * (MAX_FONT_SIZE - MIN_FONT_SIZE));
        }
        return size;
    }

    /**
     * Writes the tag cloud spans for each word to the output HTML. Each entry
     * in the list is a (word, count) pair, and the font size for each word is
     * computed from its count relative to the minimum and maximum counts in the
     * list. The list is to be sorted alphabetically by word.
     *
     * @param out
     *            the {@code PrintWriter} used to write the HTML output
     * @param words
     *            a list of {@code Map.Entry<String, Integer>} pairs, where the
     *            key is the word and the value is its frequency
     */
    private static void outputTagCloud(PrintWriter out,
            List<Map.Entry<String, Integer>> words) {

        //   find the min and max using the sorted words
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for (Map.Entry<String, Integer> x : words) {
            int count = x.getValue();
            //clear words and fill into temp
            if (count < min) {
                min = count; //  if min is more, its count
            }
            if (count > max) {
                max = count; // if max is less, its count
            }
        }
        // if no words were provided, return none
        if (min == Integer.MAX_VALUE) {
            return;
        }
        // sorting in alphabetical order
        for (Map.Entry<String, Integer> x : words) {
            String word = x.getKey();
            int count = x.getValue();
            int fontSize = computeFontSize(count, min, max);
            // proper output with fontSize
            out.println("<span style=\"cursor:default\" class=\"f" + fontSize
                    + "\" title=\"count: " + count + "\">" + word + "</span>");
        }
    }

    /**
     * Writes the closing HTML tags.
     *
     * @param out
     *            the {@code PrintWriter} used to write the HTML output
     */
    private static void outputFooter(PrintWriter out) {
        out.println("</p>");
        out.println("</div>");
        out.println("</body>");
        out.println("</html>");
    }

    /**
     * Writes the complete HTML tag cloud page, including the header section,
     * the tag cloud spans for each word, and the closing HTML tags.
     *
     * @param out
     *            the {@code PrintWriter} used to write the HTML output
     * @param inputPath
     *            the path of the original input file, displayed in the page
     *            title
     * @param nWords
     *            the number of words included in the tag cloud
     * @param cloudWords
     *            a list of (word, count) entries to display in the tag cloud,
     *            sorted alphabetically
     */
    private static void outputIndexPage(PrintWriter out, String inputPath, int nWords,
            List<Map.Entry<String, Integer>> cloudWords) {

        outputIndexHeader(out, inputPath, nWords);

        outputTagCloud(out, cloudWords);
        outputFooter(out);
    }

    /**
     * Main.
     *
     * Main method. Prompts the user for the input file and output file then
     * produces an HTML tag cloud.
     *
     * @param args
     *            command-line arguments (not used)
     */
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        System.out.print("Enter input file path: ");
        String inputPath = in.nextLine();

        System.out.print("Enter output file path: ");
        String outputPath = in.nextLine();

        System.out.print("Enter number of words to include in the tag cloud: ");
        int nWords = in.nextInt();
        in.nextLine();
        if (nWords <= 0) {
            System.err.println("ERROR: EMPTY words");
            in.close();
            return;
        }
        // Build separator set.
        Set<Character> separators = new HashSet<>();
        String separatorStr = "\t\n\r,:;.?!-—()[]{}'\"/*&$#@^_+=<>|`~\\ ";
        generateElements(separatorStr, separators);

        // Count words
        Map<String, Integer> wordCounts = new HashMap<>();

        try (BufferedReader fileIn = new BufferedReader(new FileReader(inputPath))) {
            wordIntoMap(fileIn, wordCounts, separators);
        } catch (IOException e) {
            System.err.println("ERROR: File not read - " + e.getMessage());
            in.close();
            return;
        }

        List<Map.Entry<String, Integer>> cloudWords = sort(wordCounts, nWords);

        try (PrintWriter fileOut = new PrintWriter(
                new BufferedWriter(new FileWriter(outputPath)))) {
            outputIndexPage(fileOut, inputPath, nWords, cloudWords);
        } catch (IOException e) {
            System.err.println("ERROR: File cannot be written - " + e.getMessage());
            in.close();
            return;
        }
        in.close();
    }

}
