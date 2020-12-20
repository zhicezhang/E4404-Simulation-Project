import simpy
import collections
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d.axes3d import Axes3D

from package import demand, pricing, inventory, hurricane


def refill_every_ten_days(env, inv):
    while True:
        yield env.timeout(20)
        # refill the orange every 20 days with 30 volume
        inv.refill(env.now, 30)
        print('refill 30 oranges at t=%.2f' % env.now)


def sell_every_ten_days(env, inv):
    while True:
        yield env.timeout(10)
        # selling the orange every 10 days with 8 volume
        print('selling 8 oranges at t=%.2f' % env.now)
        inv.selling(8)

#if we use constant price
def system_constant_price():
    profit=[]
    env = simpy.Environment()

    start_time = -2
    end_time = 366
    total_day = 365
    inventory_level = np.arange(0, 120, 1)

    MAX_FRESHNESS = 100
    MAX_PRICE = 30

    #time = np.linspace(0, total_day, 10000)

    #customers arrival
    customers = demand.Customers()
    arrival_func = customers.arrival_function()
    arrival_time = customers.get_arrival()
    yield env.timeout(arrival_time)

    inv = inventory.Inventory(debug=True)
    env.process(inv.inventory_process(env))


    #price strategy
    price_strategy = pricing.Price(max_price=MAX_PRICE)

    linear = price_strategy.linear_price(inventory_level)
    constant = [price_strategy.constant_price() for _ in range(len(inventory_level))]

    #customers demand
    price = np.arange(0, MAX_PRICE, 0.01)
    freshness = np.arange(0, MAX_FRESHNESS, 0.01)
    price, freshness = np.meshgrid(price, freshness)

    demand_func = customers.demand_function()
    max_demand = demand_func(price, freshness)

    #how much profit we got
    profit.append(20*max_demand)

    env.process(refill_every_ten_days(env, inv))
    env.process(sell_every_ten_days(env, inv))
    env.run(until=365)
    return np.mean(profit)


#if we use dynamic price
def system_dynamic_price():
    profit=[]
    env = simpy.Environment()

    start_time = -2
    end_time = 366
    total_day = 365
    inventory_level = np.arange(0, 120, 1)

    MAX_FRESHNESS = 100
    MAX_PRICE = 30

    #time = np.linspace(0, total_day, 10000)

    #customers arrival
    customers = demand.Customers()
    arrival_func = customers.arrival_function()
    arrival_time = customers.get_arrival()
    yield env.timeout(arrival_time)

    inv = inventory.Inventory(debug=True)
    env.process(inv.inventory_process(env))


    #price strategy
    price_strategy = pricing.Price(max_price=MAX_PRICE)

    linear = price_strategy.linear_price(inventory_level)
    constant = [price_strategy.constant_price() for _ in range(len(inventory_level))]

    #customers demand
    price = np.arange(0, MAX_PRICE, 0.01)
    freshness = np.arange(0, MAX_FRESHNESS, 0.01)
    price, freshness = np.meshgrid(price, freshness)

    demand_func = customers.demand_function()
    max_demand = demand_func(price, freshness)

    #how much profit we got
    profit.append(linear*max_demand)

    env.process(refill_every_ten_days(env, inv))
    env.process(sell_every_ten_days(env, inv))
    env.run(until=365)
    return np.mean(profit)

print(system_constant_price())
