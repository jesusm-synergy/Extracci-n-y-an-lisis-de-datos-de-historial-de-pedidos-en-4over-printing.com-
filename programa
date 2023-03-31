import requests
from bs4 import BeautifulSoup

def extract_order_info(soup):
    order_items = soup.find_all("div", class_="order-item_order")
    
    orders_data = []
    for order_item in order_items:
        order_number = order_item.select_one(".order-head_number span").text
        ship_to = order_item.select_one(".order-head_ship-to span").text
        shipping_method = order_item.select_one(".order-head_shipping-method span").text
        jobs = order_item.select_one(".order-head_jobs span").text
        order_total = order_item.select_one(".order-head_total span").text.strip()
        
        sets_list = order_item.select(".order_sets-list .set-item_row")
        
        for set_item in sets_list:
            set_name = set_item.select_one(".set-item_cell.set-item_name span").text
            set_status = set_item.select_one(".set-item_cell.set-item_status span").text

            orders_data.append({
                "Order Number": order_number,
                "Ship To": ship_to,
                "Shipping Method": shipping_method,
                "Jobs": jobs,
                "Order Total": order_total,
                "Set Name": set_name,
                "Set Status": set_status
            })
    
    return orders_data

def main():
    # Reemplaza estos valores con tus propias credenciales
    username = "tu_nombre_de_usuario"
    password = "tu_contraseña"

    # URL de inicio de sesión y datos del formulario
    login_url = "https://4over-printing.com/customer/account/loginPost/"
    payload = {
        "login[username]": username,
        "login[password]": password
    }

    # Iniciar sesión en el sitio web
    session = requests.Session()
    response = session.post(login_url, data=payload)

    if response.status_code == 200:
        print("Inicio de sesión exitoso")
    else:
        print(f"Error en el inicio de sesión. Código: {response.status_code}")
        return

    # Cambia este valor para controlar cuántas páginas de historial de pedidos deseas extraer
    num_pages = 3

    all_orders_data = []
    for page in range(1, num_pages + 1):
        orders_url = f"https://4over-printing.com/sales/order/history/?advanced-filtering=&date-range=&job-status=&order-date-from=&order-date-to=&p={page}&search-term="
        response = session.get(orders_url)

        if response.status_code == 200:
            soup = BeautifulSoup(response.text, "html.parser")
            orders_data = extract_order_info(soup)
            all_orders_data.extend(orders_data)
            print(f"Datos extraídos de la página {page}")
        else:
            print(f"Error al acceder a la página de historial de pedidos. Código: {response.status_code}")

    # Imprimir la información extraída
    for order_data in all_orders_data:
        for key, value in order_data.items():
            print(f"{key}: {value}")
        print("\n")

if __name__ == "__main__":
    main()
