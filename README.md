# Desenvolvimento-da-area-de-gerenciamento-de-agenda

<!DOCTYPE html>
<html>
<head>
	<title>Agenda </title>
</head>
<body>
	<h1>Agenda </h1>
	<h2>Agenda para o dia</h2>
	<table>
		<tr>
			<th>Horário</th>
			<th>Cliente</th>
			<th>Observações</th>
			<th>Opções</th>
		</tr>
		<tr>
			<td>09:00</td>
			<td>João da Silva</td>
			<td>Corte de cabelo e barba</td>
			<td>
				<button>Confirmar</button>
				<button>Cancelar</button>
			</td>
		</tr>
		<tr>
			<td>10:00</td>
			<td>Maria Oliveira</td>
			<td>Corte de cabelo</td>
			<td>
				<button>Confirmar</button>
				<button>Cancelar</button>
			</td>
		</tr>
	</table>
</body>
</html>

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class AgendaBarbeiro {
	private Connection connection;

	public AgendaBarbeiro() throws SQLException {
		connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/agendabarbeiro", "username", "password");
	}

	public ResultSet getAgendaDoDia() throws SQLException {
		PreparedStatement statement = connection.prepareStatement("SELECT horario, cliente, observacoes FROM agenda WHERE data = ?");
		statement.setDate(1, java.sql.Date.valueOf(java.time.LocalDate.now()));
		return statement.executeQuery();
	}

	public void confirmarReserva(int idReserva) throws SQLException {
		PreparedStatement statement = connection.prepareStatement("UPDATE agenda SET confirmado = true WHERE id = ?");
		statement.setInt(1, idReserva);
		statement.executeUpdate();
	}

	public void cancelarReserva(int idReserva) throws SQLException {
		PreparedStatement statement = connection.prepareStatement("UPDATE agenda SET cancelado = true WHERE id = ?");
		statement.setInt(1, idReserva);
		statement.executeUpdate();
	}
}
