import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class MathUtilsTest {

    private MathUtils mathUtils;

    @BeforeEach
    void setUp() {
        mathUtils = new MathUtils();
    }

    @AfterEach
    void tearDown() {
        mathUtils = null;
    }

    // ---- add() tests ----
    @Test
    void testAddPositiveNumbers() {
        assertEquals(5, mathUtils.add(2, 3));
    }

    @Test
    void testAddWithNegativeNumbers() {
        assertEquals(-1, mathUtils.add(2, -3));
    }

    // ---- subtract() tests ----
    @Test
    void testSubtractPositiveNumbers() {
        assertEquals(1, mathUtils.subtract(3, 2));
    }

    @Test
    void testSubtractWithNegativeResult() {
        assertEquals(-5, mathUtils.subtract(2, 7));
    }

    // ---- multiply() tests ----
    @Test
    void testMultiplyPositiveNumbers() {
        assertEquals(6, mathUtils.multiply(2, 3));
    }

    @Test
    void testMultiplyByZero() {
        assertEquals(0, mathUtils.multiply(5, 0));
    }

    @Test
    void testMultiplyWithNegativeNumber() {
        assertEquals(-6, mathUtils.multiply(2, -3));
    }

    // ---- divide() tests ----
    @Test
    void testDivideValidNumbers() {
        assertEquals(2.5, mathUtils.divide(5, 2), 0.0001);
    }

    @Test
    void testDivideByZero() {
        assertEquals(-1.0, mathUtils.divide(5, 0));
    }

    @Test
    void testDivideNegativeNumbers() {
        assertEquals(-2.0, mathUtils.divide(4, -2), 0.0001);
    }
}
