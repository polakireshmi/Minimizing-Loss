# Minimizing-Loss
import bisect

def find_min_loss(prices):
    """
    Finds the minimum loss from buying and selling a house at a loss.
    
    Args:
        prices: List of house prices over years (index represents year)
    
    Returns:
        Tuple of (buy_year, sell_year, min_loss)
    """
    min_loss = float('inf')
    buy_year = sell_year = -1
    sorted_prices = []
    
    # We'll process prices in reverse order (from last year to first)
    for current_year in range(len(prices)-1, -1, -1):
        current_price = prices[current_year]
        
        # Find the first price in sorted list that's >= current_price
        idx = bisect.bisect_left(sorted_prices, current_price)
        
        if idx < len(sorted_prices):
            # Potential sell price found
            loss = sorted_prices[idx] - current_price
            if 0 < loss < min_loss:
                min_loss = loss
                buy_year = current_year
                # Find the earliest possible sell year with this price
                sell_year = prices.index(sorted_prices[idx], current_year + 1)
        
        # Insert current price into the sorted list
        bisect.insort(sorted_prices, current_price)
    
    return (buy_year + 1, sell_year + 1, min_loss)  # Converting to 1-based indexing

# Example usage
if __name__ == "__main__":
    prices = [20, 15, 7, 2, 13]
    buy_year, sell_year, loss = find_min_loss(prices)
    print(f"Buy in year {buy_year} at {prices[buy_year-1]}")
    print(f"Sell in year {sell_year} at {prices[sell_year-1]}")
    print(f"Minimum loss: {loss}")
