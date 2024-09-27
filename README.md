# lagranges
import java.util.HashMap;
import java.util.Map;

public class PolynomialSolver {


    public static Map<Integer, Long> decodeYValues(Map<Integer, Map<String, String>> encodedValues) {
        Map<Integer, Long> decodedValues = new HashMap<>();
        for (Map.Entry<Integer, Map<String, String>> entry : encodedValues.entrySet()) {
            int key = entry.getKey();
            int base = Integer.parseInt(entry.getValue().get("base"));
            long decodedValue = Long.parseLong(entry.getValue().get("value"), base);
            decodedValues.put(key, decodedValue);
        }
        return decodedValues;
    }


    public static double lagrangeInterpolation(int[] xValues, long[] yValues) {
        int n = xValues.length;
        double c = 0;

        for (int j = 0; j < n; j++) {
            double term = yValues[j];
            for (int m = 0; m < n; m++) {
                if (m != j) {
                    term *= (0 - xValues[m]) / (double) (xValues[j] - xValues[m]);
                }
            }
            c += term;
        }
        return c;
    }

    public static void main(String[] args) {
  
        Map<Integer, Map<String, String>> data1 = new HashMap<>();
        data1.put(1, Map.of("base", "10", "value", "4"));
        data1.put(2, Map.of("base", "2", "value", "111"));
        data1.put(3, Map.of("base", "10", "value", "12"));
        data1.put(6, Map.of("base", "4", "value", "213"));

        // Provided JSON data for the second test case
        Map<Integer, Map<String, String>> data2 = new HashMap<>();
        data2.put(1, Map.of("base", "10", "value", "28735619723837"));
        data2.put(2, Map.of("base", "16", "value", "1A228867F0CA"));
        data2.put(3, Map.of("base", "12", "value", "32811A4AA0B7B"));
        data2.put(4, Map.of("base", "11", "value", "917978721331A"));
        data2.put(5, Map.of("base", "16", "value", "1A22886782E1"));
        data2.put(6, Map.of("base", "10", "value", "28735619654702"));
        data2.put(7, Map.of("base", "14", "value", "71AB5070CC4B"));
        data2.put(8, Map.of("base", "9", "value", "122662581541670"));
        data2.put(9, Map.of("base", "8", "value", "642121030037605"));

      
        for (Map<Integer, Map<String, String>> data : new Map[]{data1, data2}) {

            Map<Integer, Long> decodedYValues = decodeYValues(data);

           
            int[] xValues = decodedYValues.keySet().stream().mapToInt(Integer::intValue).toArray();
            long[] yValues = decodedYValues.values().stream().mapToLong(Long::longValue).toArray();

           
            double c = lagrangeInterpolation(xValues, yValues);

           
            System.out.println("c = " + c);
        }
    }
}
