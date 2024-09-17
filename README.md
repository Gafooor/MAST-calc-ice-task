import * as React from 'react';
import { Text, View, TouchableOpacity, StyleSheet } from 'react-native';

export default function App() {
  const [displayValue, setDisplayValue] = React.useState('0');
  const [operator, setOperator] = React.useState(null);
  const [firstValue, setFirstValue] = React.useState(null);
  const [waitingForSecondValue, setWaitingForSecondValue] = React.useState(false);

  const handleTap = (type, value) => {
    if (type === 'number') {
      setDisplayValue((prevDisplayValue) => {
        if (waitingForSecondValue) {
          setWaitingForSecondValue(false);
          return `${value}`;
        }
        return prevDisplayValue === '0' ? `${value}` : `${prevDisplayValue}${value}`;
      });
    }

    if (type === 'operator') {
      setOperator(value);
      setFirstValue(parseFloat(displayValue));
      setWaitingForSecondValue(true);
    }

    if (type === 'equal') {
      const secondValue = parseFloat(displayValue);
      if (operator === '+') {
        setDisplayValue(String(firstValue + secondValue));
      } else if (operator === '-') {
        setDisplayValue(String(firstValue - secondValue));
      } else if (operator === '*') {
        setDisplayValue(String(firstValue * secondValue));
      } else if (operator === '/') {
        setDisplayValue(String(firstValue / secondValue));
      }
      setOperator(null);
      setFirstValue(null);
      setWaitingForSecondValue(false);
    }

    if (type === 'clear') {
      setDisplayValue('0');
      setOperator(null);
      setFirstValue(null);
      setWaitingForSecondValue(false);
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.display}>{displayValue}</Text>
      <View style={styles.row}>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('clear')}>
          <Text style={styles.buttonText}>C</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('operator', '/')}>
          <Text style={styles.buttonText}>/</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('operator', '*')}>
          <Text style={styles.buttonText}>*</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('operator', '-')}>
          <Text style={styles.buttonText}>-</Text>
        </TouchableOpacity>
      </View>
      <View style={styles.row}>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 7)}>
          <Text style={styles.buttonText}>7</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 8)}>
          <Text style={styles.buttonText}>8</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 9)}>
          <Text style={styles.buttonText}>9</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('operator', '+')}>
          <Text style={styles.buttonText}>+</Text>
        </TouchableOpacity>
      </View>
      <View style={styles.row}>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 4)}>
          <Text style={styles.buttonText}>4</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 5)}>
          <Text style={styles.buttonText}>5</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 6)}>
          <Text style={styles.buttonText}>6</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('equal')}>
          <Text style={styles.buttonText}>=</Text>
        </TouchableOpacity>
      </View>
      <View style={styles.row}>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 1)}>
          <Text style={styles.buttonText}>1</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 2)}>
          <Text style={styles.buttonText}>2</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 3)}>
          <Text style={styles.buttonText}>3</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.button} onPress={() => handleTap('number', 0)}>
          <Text style={styles.buttonText}>0</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    backgroundColor: '#F5FCFF',
  },
  display: {
    fontSize: 40,
    fontWeight: 'bold',
    textAlign: 'right',
    padding: 20,
    backgroundColor: '#fff',
  },
  row: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginVertical: 10,
  },
  button: {
    backgroundColor: '#ddd',
    padding: 20,
    flex: 1,
    marginHorizontal: 5,
    alignItems: 'center',
  },
  buttonText: {
    fontSize: 20,
    fontWeight: 'bold',
  },
});
